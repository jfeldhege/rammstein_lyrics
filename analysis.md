Analysis of Rammstein lyrics
================
Johannes Feldhege
11 11 2021

## Load the necessary packages

``` r
library(spotifyr)
library(dplyr)
library(stringr)
library(ggplot2)
```

## Access the Spotify API and download all albums for Rammstein.

The client id and secret for the Spotify API are saved in my R
environment and are called with Sys.getenv()

``` r
access_token <- get_spotify_access_token(client_id = Sys.getenv("SPOTIFY_CLIENT_ID"),
  client_secret = Sys.getenv("SPOTIFY_CLIENT_SECRET"))

rammstein_albums <- get_artist_audio_features('rammstein',
                                     include_groups = c("album"))
```

## How many albums do we have?

``` r
rammstein_albums %>% 
  count(album_name, album_id)
```

    ##                                          album_name               album_id  n
    ## 1  Herzeleid (XXV Anniversary Edition – Remastered) 6HlDm9fzYxk4vkkr3YEvSv 11
    ## 2                             LIEBE IST FÜR ALLE DA 3pksjSyo1w4vAPOenAzM4Z 16
    ## 3           LIEBE IST FÜR ALLE DA (SPECIAL EDITION) 29J0MchD0OWNeiMNbPDwfa 16
    ## 4                                   Live aus Berlin 0P3nXUvcows985g3qGA8mm 15
    ## 5                                   Live aus Berlin 5pCA8SReb0Slsify6rsb7h 15
    ## 6                                            Mutter 1CtTTpKbHU8KbHRB4LmBbv 11
    ## 7                                            Mutter 7ikuEzL6xeAgu6yT6YVLy7 11
    ## 8                                      PARIS (LIVE) 2SU0P5F7ZSN36pUZeYgAAG 22
    ## 9                                      PARIS (LIVE) 75kG9OU9FwAd4HQOLylOB1 22
    ## 10                                        RAMMSTEIN 1LoyJQVHPLHE3fCCS8Juek 11
    ## 11                                     Reise, Reise 09qHS2BgOLqi3SMkbauxdJ 11
    ## 12                                     REISE, REISE 74ydDCcXTco741y42ceRJ5 11
    ## 13                                         ROSENROT 3PrS5X3QNdDUlBIE4bz6J2 11
    ## 14                                         ROSENROT 3ujsh5jQMOFqIbP7sWruEt 11
    ## 15                                        Sehnsucht 0C9p8YMtbdOkcXPPlEmZvY 11
    ## 16                                        Sehnsucht 2w6Vy8qZLU4niyQAHyu0Ag 11
    ## 17                                    XXI - Klavier 6rK4qSVdrNCNnQwRCEFsw9 13

For some reasons, some of the albums appear twice in the dataset. Since
I only want to use the studio albums, I have to select unique albums by
album id. I will also remove unneeded columns, arrange the albums by
release year and change the album names to sentence case.

``` r
studio_album_ids <- c("6HlDm9fzYxk4vkkr3YEvSv","3pksjSyo1w4vAPOenAzM4Z",
                      "1CtTTpKbHU8KbHRB4LmBbv","1LoyJQVHPLHE3fCCS8Juek",
                      "09qHS2BgOLqi3SMkbauxdJ", "3PrS5X3QNdDUlBIE4bz6J2",
                      "0C9p8YMtbdOkcXPPlEmZvY")

rammstein_albums <- rammstein_albums %>% 
  filter(album_id %in% studio_album_ids) %>% 
  select(track_name, album_name, album_id, danceability:tempo, duration_ms,
         album_release_year,track_number) %>% 
  arrange(album_release_year, track_number) %>% 
  mutate(album_name = str_to_title(str_remove(album_name, " \\(.+\\)")),
         track_name = str_to_title(track_name))

head(rammstein_albums)
```

    ##                            track_name album_name               album_id
    ## 1 Wollt Ihr Das Bett In Flammen Sehen  Herzeleid 6HlDm9fzYxk4vkkr3YEvSv
    ## 2                         Der Meister  Herzeleid 6HlDm9fzYxk4vkkr3YEvSv
    ## 3                     Weisses Fleisch  Herzeleid 6HlDm9fzYxk4vkkr3YEvSv
    ## 4                      Asche Zu Asche  Herzeleid 6HlDm9fzYxk4vkkr3YEvSv
    ## 5                             Seemann  Herzeleid 6HlDm9fzYxk4vkkr3YEvSv
    ## 6                   Du Riechst So Gut  Herzeleid 6HlDm9fzYxk4vkkr3YEvSv
    ##   danceability energy key loudness mode speechiness acousticness
    ## 1        0.673  0.766   2   -7.342    1      0.0394     2.37e-05
    ## 2        0.661  0.699   9   -7.982    1      0.0369     7.07e-06
    ## 3        0.603  0.750  10   -9.282    1      0.0378     9.40e-06
    ## 4        0.563  0.906   9   -7.503    1      0.0734     1.64e-03
    ## 5        0.600  0.634   4  -10.699    1      0.0442     3.03e-03
    ## 6        0.676  0.747   3   -7.732    1      0.0308     1.11e-04
    ##   instrumentalness liveness valence   tempo duration_ms album_release_year
    ## 1           0.6730   0.6990   0.641 129.928      315850               1995
    ## 2           0.6660   0.1770   0.629 130.144      250558               1995
    ## 3           0.0839   0.0667   0.611 132.107      215998               1995
    ## 4           0.0179   0.0722   0.710 169.967      231449               1995
    ## 5           0.0192   0.1010   0.246 139.932      287857               1995
    ## 6           0.2030   0.3560   0.797 111.850      291660               1995
    ##   track_number
    ## 1            1
    ## 2            2
    ## 3            3
    ## 4            4
    ## 5            5
    ## 6            6

Now that we have the albums and columns that we want we can start
visualising.

## Valence
