# MovieColorPalette-TwitterBot

A Python based project which dynamically gets movie posters from The movie Database(TMDb) API. It then converts the poster path of the particular movie to an actual image that the program can work with.
Then it generates the color palette form the image using K means clustering. It then vertically combines the two images - the original poster and its newly generated color pallete and tweets the newly generated image out. The program can be customised to tweet at particular time. 
For me my poster tweets a new image out once a day. You can see and follow my project live here- https://twitter.com/CodeNeon1


# Get the Data:

My first plan was to build a local database where i manually enter the posters so my program can access it. But i decided it would be more efficient for me to use an API. 
That way i would not have to manually enter data for the new movies that came out and make my code more effective and dynamic. so i researched and found TMDb API. which is a large online dynamic database for all the Movies and TV shows.

First it's important to import all the necessary libraries: 

    import requests
    import numpy as np
    import cv2
    import urllib.request
    from DominantColors import DominantColors
    import urllib
    import tweepy
    import time


This is the code which gets the data and the poster path for the movie. After registering and getting your API key we initiate few variables. To understand better the way to call the movies or its images and other data it's best to read through the documentation of TMDb API.

When i ran the program i found that the database does not has all the index numbers for movies and many are missing. This causes errors when the program is run thus a exception handling is implied in the for loop. Now if an movie index is missing in the database, then it would skip that movie index number and go to the next one.

urls = [], is an empty  list which would contain the url for each movie’s poster path.  This would be used later to extract the image data to e used for K-means clustering processing. 


     # Twitter bot initialization
    consumer_key = 'CONSUMER_KEY'
    consumer_secret = 'CONSUMER_SECRET'
    access_token = 'ACCESS_TOKEN'
    access_token_secret = 'ACCESS_TOKEN_SECRET'

    auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
    auth.set_access_token(access_token, access_token_secret)
    api = tweepy.API(auth)


    # Variables
    api_key = '?YOUR_API_KEY'
    url = 'https://api.themoviedb.org/3/movie/'
    urls = []
    start = 0



    for i in range(start,end):
            try:
                mov_num = str(i)
                JSONcontent = requests.get(url + mov_num + api_key)
                response = JSONcontent.json()

                base_url = 'https://image.tmdb.org/t/p/w500'
                img_url = response['poster_path']
                image_url = base_url + img_url

                urls.append(image_url)
                print(urls)

            except:

                print("pass")
                pass
             
  
  
# Convert web data to array of data

This is a function which actually converts each ur from our urls[] list into a image data array that our program can easily read and perform functions on. 


      def url_to_image(url):
        resp = urllib.request.urlopen(url)
        image = np.array(bytearray(resp.read()))

        image = cv2.imdecode(image, cv2.IMREAD_COLOR)

        return image
 
 
# Generating Color Palette + Twitter Bot

The for loop converts each poster image url path into a data array that can be used by the program. Here the previously defined function  - def url_to_image(url): - Is used to get the image data that would be parsed to  the DominantColors() class. That particular class is a K-means clustering algorithm which generates the color pallete.
Then both this newly generated image and  the color pallete is combined in one image and using the simple twitter bot it's twttted out. Between each tweet, a time.sleep() is used. To tweet at a particular time.


    for url in urls:
    image = url_to_image(url)

    cv2.imwrite("1.jpg", image)

    img = '1.jpg'
    clusters = 6

    dc = DominantColors(img, clusters)
    dc.dominantColors()
    color = dc.plotHistogram()

    numpy_vertical = np.vstack((color, image))
    cv2.imwrite("main.jpg", numpy_vertical)
    final = "main.jpg"
 
    
    #Twitter Bot

    time.sleep(60)
    api.update_with_media(final, "Follow my code!  -  https://github.com/DataStroke99/MovieColorPalette-TwitterBot "
                                 + " #cinema #movie #moviepalette #palette #MovieColor #color #art #computerscience")











