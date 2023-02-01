# Spotify API

The goal of this repository, will be to:

- **Understand how the Spotify-API works**,
- **Get music data by using the Spotify-API** in Python,
- Use the scraped data to **make a simple exploratory data analysis**.

_The tutorial for this repository was inspired by this job-add: https://uzhcareer.ch/de/jobs/detail/?id=c95ddda0-969d-ed11-810d-f76dba3d4646_

## Step 1: Create a Virtual Environment (`venv`)

I will create a virtual environment for this project and work form there:

1) In your terminal, type in the following (make sure that you are inside your `wd`!): `python3 -m venv Your_Env_Name`. This will create the virtual-environment.
2) Next, you need to **activate this environment**: 
    - If you use your Mac: `source Your_Env_Name/bin/activate`.
    - If you are on a **Windows**-Computer: `Your_Env_Name\Scripts\activate.bat`
3) Check if it worked: `python --version`

That's it for the installation of the virtual environment!

---
#### <u>Bemerkung</u>: Achtung bei der Änderung des Projekt-Pfades!
---

**Wenn du den Namen eines Ordners änderst, der auf dem Pfad des `venv` liegt, dann wirst du die <ins>falsche </ins> Python-Version runnen (bei mir war es `Python 2.7.16` statt `Python 3.8.2`, welche die korrekte wäre!**

- <u>To fix this</u>: 

    1) Follow this path: `path_to_your_env/bin/activate`, während du dich in deinem Projekt-Ordner befindest (= Annahme: der Projekt-Ordner sollte dein `wd` sein).
    2) Öffne die `activate`-Datei. Dort musst du folgende Zeile ändern:
    ```
    VIRTUAL_ENV="/Users/jomaye/Documents/Programming/Python/projects/libraries-to-try/statsmodels/env-statmodels"
    ```
    zu...:
    ```
    VIRTUAL_ENV="/Users/jomaye/Documents/Programming/Python/projects/data-science/statsmodels/env-statmodels"
    ```

_Wie du anhand des obigen Beispieles siehst, war das Problem, dass ich **den Ordner** `libraries-to-try` auf `data-science` umbenannt hatte. Somit war der **Pfad** zum `venv` falsch und hat dazu geführt, dass der Computer automatisch auf die Default-Python-Version `2.7.16`, welche auf meinem Computer installiert ist, gewechselt ist (statt auf die `Python 3.8.2` Version, die ich eigentlich wollte...)_.

- <u>To check if it worked</u>: `python --version` im Terminal eingeben. 
    - Wenn nun `Python 3.8.2` steht, dann hast du das Problem gelöst! =)

### Update `pip`

Before I start with the project, I will make sure that my `pip`-installer is up-to-date. **I am currently using the version `22.2.2` (Stand: 28.09.2022) for this project**.

- To check your current version of `pip`, simply type: `pip --version`.
- In order to upgrade `pip`, use the following command: `pip install --upgrade pip`

> Why should `pip` be up-to-date?: 

*If you’re using an old version of pip, installing the latest version of a Python package might fail—or install in a slower, more complex way*. 

### Install the packages

Now you can install the package:

1) `pip install jupyter`
    > For the **exact** Version I used, run the following command instead: `pip install jupyter==1.0.0`
    > To run a Jupyter Notebook - after your `venv` has been activated - simply use `jupyter notebook` in your terminal.
2) `pip install pandas`
    > I need this package to load `.csv`-files, as well as to make some data processing.
3) `pip install spotipy`
    > I need this package to make a connection to the Spotify-API
4) `pip install seaborn matplotlib geopy folium holoviews`
    > Those are all the packages I needed for data visualization purposes. 


## Roadmap

- In their job-description (link: https://uzhcareer.ch/de/jobs/detail/?id=c95ddda0-969d-ed11-810d-f76dba3d4646), Sony mentions that - one part of the job - will revolve around analyzing music-trends. Hence, we would need to find data on music-trends. Grouped by countries would be very nice! And which Tech-Company is specialized in music? Spotify. Luckily, they offer a service of using their API FOR FREE, if it is for non-commercial pourposes! 
- Next, I needed to create a Spotify-Account to access their API: https://www.youtube.com/watch?v=xyrLcTiMbgc&t=2m6s
- In order to understand how the Spotify-API works, we need to read the docs. By reading through those resources, I realized that I needed to know the URI (= "uniform resource identifier") for EACH country (link: https://developer.spotify.com/documentation/web-api/#spotify-uris-and-ids).
    - First, we therefore need to know each country that is available in the Spotify API. You should be able to retrieve each of the country's URIs on the Spotify-Website, but you need to log-in first, in order to retrieve all the countries' names (link: https://charts.spotify.com/home). Once you followed the link, simply paste each country name in the "search" field (via this link: https://open.spotify.com/search) and - then - in the URL of your browser, the long string with random characters is the URI. This is what we need to create a list of all URIs that we will use...
- Next, we will need to establish a connection to the Spotify-API. 
    - We need 2 things here: 1) a Private- & Public-API that is generated by the Spotify-API; 2) We also need to use the Package `spotipy` to get a simplified access to all the Spotify-Data.
        - To retrieve the Public- & Private-Key (= to authorize ourself into the Spotify-API), we simply need to klick on "Create new App" on this link: https://developer.spotify.com/dashboard/
        - In order to use `spotipy`, we simply need to be in our virtual environment and then use `pip install spotipy` (link to docs: https://spotipy.readthedocs.io/en/2.22.1/).
- Now, we will need to create the dataset. For this, I found a really nice coding-example from towards-datascience (https://towardsdatascience.com/extracting-song-data-from-the-spotify-api-using-python-b1e79388d50), where there was a showcase of how to retrieve the data from the "Global Top 50"-Songs. 
    - After that, I basically needed to write a for-loop which would iterate over every of the 63 URIs of all Top 50 from each country. I tested it for 2 URIs and - after that worked - I tried to do the whole for-loop. 
    - When trying to do the whole for-loop, I first got a notification that the "request" to the Spotify Web-API timed out. That's why I increased the request-time when creating the Spotify-Class. Then I tried again, and - after about 5 minutes - it worked! I have my dataset ready
        - The 5 minutes waiting was scary, because - at first - I thought that I made wayyy to many requests and that Spotify may ban me!! If you do this kind of stuff at a company, be sure to not violate their terms of use and read through everything carefully. You can risk to pay high fees if you do such things!!! Also, avoid writing an infite loop (always test out everything before you run the ACTUAL big for-loop...)!

## Important Things I learned

- It took me approximately 9 hours (= 1 full day of work) to retrieve the data from the Spotify Web-API.
- When working with APIs, you simply ask the API to get access to different Tables with Data. Here, the key-point is to know that there are "unique IDs" ACROSS all those tables, such that - when you create your own dataset - you can combine the information you need across all those different tables. For example, when I constructed my dataset, some information was 'hidden' in the "Artists" table (like Artist Genre or Artist Popularity, as well as the unique ID of the Artist), while other data was in the "Playlist" table (like Name of the Song, Name of the Artist who sings the song, the Artist's unique ID etc...).
    - Remember: In the Spotify Web-Api, they called unique IDs "URIs". This is simply a random string of 22 characters (numbers, as well as text all binded together).

_Thank you for reading. Hope it helps!_