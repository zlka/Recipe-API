# Recipe-API
The user inputer an ingredient. The code will search for all recipes given that ingredient from an API.

import requests 

#Search by ingredient.
def recipe_search(ingredient): 
    # Register to get an APP ID and key toma 
    app_id = '1015b6e6' 
    app_key = '5d0ba584cd71a281b1e4bc688014f536' 
    result = requests.get('https://api.edamam.com/search?q={}&app_id={}&app_key={}'.format(ingredient, app_id, app_key) ) 
    data = result.json() 
 
    return data['hits'] 

#This is to print out the recipe's name and link. Avoids repeatition
def print_output(recipe):
    print(recipe['label']) 
    print(recipe['url']) 
    print()

def save_txt():
    from urllib.request import urlopen   #Reads the link
    import html2text #changes the website format to txt
    wish=input('Do you want to save a recipe into a txt file?: ')
    if wish =='Yes':
        url=input('Place the link of the recipe you want to download: ')
        html = urlopen(url).read().decode('utf-8')
        rendered_content = html2text.html2text(html)
        file = open('Recipe.txt', 'w')
        file.write(rendered_content)
        file.close()
        print('Your file has been saved successfully as "Recipe.txt"')
    else:
        if wish == 'No':
            print('OK')
        else: 
            print('Your response was invalid. Your search has ended')

#Global Variable: this doesnt change
#!!!!!!!!!!!!!!!!!!!!!!!!!Add all labels of the API. !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
formal_requestlist = ['Vegetarian','No'] 

 
def run(): 
    ingredient = input('Enter an ingredient: ') 
    #Check the ingredient presence inside the recipe
    results = recipe_search(ingredient)  
    #Check if the user have a request 
    requests = input('Type any special request: // Type "No" otherwise.')    

    '''
    To check the labels in general. Some recipes have some labels that others don't so we create a list and check 
    against that list to see if the request is valid. 
    '''

    while requests not in formal_requestlist:     #While loop to stop the user from continuing 
        print('Your request is not valid, The options available are:')
        print(formal_requestlist)
        requests = input('Do you want to type a new request?: ')

    for result in results: 
        recipe = result['recipe'] 
        if requests!='No':
            #This is for checking each element's label
            requestlist = recipe['dietLabels'] + recipe['healthLabels'] 
            if requests in requestlist:
                print_output(recipe)
                
        else:
            print_output(recipe)
    save_txt()
            


run() 
 
 
  
