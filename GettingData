import requests
import numpy as np
import cv2
import urllib.request
from DominantColors import DominantColors
import urllib
import tweepy
import time

# Twitter bot initialization
consumer_key = ''
consumer_secret = ''
access_token = ''
access_token_secret = ''

auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
auth.set_access_token(access_token, access_token_secret)
api = tweepy.API(auth)


#variables
api_key = '?API_KEY_HERE'
url = 'https://api.themoviedb.org/3/movie/'
urls = []
start = 0
end = 100

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



def url_to_image(url):
    resp = urllib.request.urlopen(url)
    image = np.array(bytearray(resp.read()))

    image = cv2.imdecode(image, cv2.IMREAD_COLOR)

    return image




for url in urls:
    image = url_to_image(url)

    # cv2.imshow("Image", image)
    cv2.imwrite("1.jpg", image)

    img = '1.jpg'
    clusters = 6

    dc = DominantColors(img, clusters)
    dc.dominantColors()
    color = dc.plotHistogram()
    # cv2.imshow("img", color)

    numpy_vertical = np.vstack((color, image))
    # cv2.imshow('main_image', numpy_vertical)
    cv2.imwrite("main.jpg", numpy_vertical)
    final = "main.jpg"
    # cv2.waitKey()


    #Twitter Bot

    time.sleep(60)
    api.update_with_media(final, "Follow my code!  -  https://github.com/DataStroke99/MovieColorPalette-TwitterBot "
                                 + " #cinema #movie #moviepalette #palette #MovieColor #color #art #computerscience")




