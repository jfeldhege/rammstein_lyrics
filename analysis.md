Analysis of Rammstein lyrics
================
Johannes Feldhege
11 11 2021

## Load the necessary packages

    ## 
    ## Attache Paket: 'dplyr'

    ## Die folgenden Objekte sind maskiert von 'package:stats':
    ## 
    ##     filter, lag

    ## Die folgenden Objekte sind maskiert von 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

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
  mutate(album_name = str_to_title(album_name))

rammstein_albums
```

    ##                                     track_name
    ## 1          Wollt ihr das Bett in Flammen sehen
    ## 2                                  Der Meister
    ## 3                              Weisses Fleisch
    ## 4                               Asche zu Asche
    ## 5                                      Seemann
    ## 6                            Du riechst so gut
    ## 7                                Das alte Leid
    ## 8                                 Heirate mich
    ## 9                                    Herzeleid
    ## 10                                   Laichzeit
    ## 11                                   Rammstein
    ## 12                                   Sehnsucht
    ## 13                                       Engel
    ## 14                                        Tier
    ## 15                               Bestrafe mich
    ## 16                                     Du hast
    ## 17                                   Bück dich
    ## 18                               Spiel mit mir
    ## 19                                     Klavier
    ## 20                                  Alter Mann
    ## 21                                  Eifersucht
    ## 22                      Küss mich (Fellfrosch)
    ## 23                            Mein Herz brennt
    ## 24                                 Links 2 3 4
    ## 25                                       Sonne
    ## 26                                    Ich will
    ## 27                                 Feuer frei!
    ## 28                                      Mutter
    ## 29                                    Spieluhr
    ## 30                                     Zwitter
    ## 31                                   Rein raus
    ## 32                                       Adios
    ## 33                                       Nebel
    ## 34                                REISE, REISE
    ## 35                                   MEIN TEIL
    ## 36                                  DALAI LAMA
    ## 37                                  KEINE LUST
    ## 38                                         LOS
    ## 39                                     AMERIKA
    ## 40                                      MOSKAU
    ## 41                                 MORGENSTERN
    ## 42                              STEIN UM STEIN
    ## 43                                   OHNE DICH
    ## 44                                       AMOUR
    ## 45                                      BENZIN
    ## 46                             MANN GEGEN MANN
    ## 47                                    ROSENROT
    ## 48                                      SPRING
    ## 49                                  WO BIST DU
    ## 50 STIRB NICHT VOR MIR (DON'T DIE BEFORE I DO)
    ## 51                                   ZERSTÖREN
    ## 52                                    HILF MIR
    ## 53                             TE QUIERO PUTA!
    ## 54                            FEUER UND WASSER
    ## 55                                    EIN LIED
    ## 56                                    RAMMLIED
    ## 57                              ICH TU DIR WEH
    ## 58                              WAIDMANNS HEIL
    ## 59                                    HAIFISCH
    ## 60                                   B********
    ## 61                           FRÜHLING IN PARIS
    ## 62                                 WIENER BLUT
    ## 63                                       PUSSY
    ## 64                       LIEBE IST FÜR ALLE DA
    ## 65                                        MEHR
    ## 66                                  ROTER SAND
    ## 67                                  FÜHRE MICH
    ## 68                                 DONAUKINDER
    ## 69                                        HALT
    ## 70              ROTER SAND - ORCHESTER VERSION
    ## 71                                       LIESE
    ## 72                                 DEUTSCHLAND
    ## 73                                       RADIO
    ## 74                                   ZEIG DICH
    ## 75                                   AUSLÄNDER
    ## 76                                         SEX
    ## 77                                       PUPPE
    ## 78                               WAS ICH LIEBE
    ## 79                                     DIAMANT
    ## 80                                    WEIT WEG
    ## 81                                      TATTOO
    ## 82                                   HALLOMANN
    ##                                          album_name               album_id
    ## 1  Herzeleid (Xxv Anniversary Edition – Remastered) 6HlDm9fzYxk4vkkr3YEvSv
    ## 2  Herzeleid (Xxv Anniversary Edition – Remastered) 6HlDm9fzYxk4vkkr3YEvSv
    ## 3  Herzeleid (Xxv Anniversary Edition – Remastered) 6HlDm9fzYxk4vkkr3YEvSv
    ## 4  Herzeleid (Xxv Anniversary Edition – Remastered) 6HlDm9fzYxk4vkkr3YEvSv
    ## 5  Herzeleid (Xxv Anniversary Edition – Remastered) 6HlDm9fzYxk4vkkr3YEvSv
    ## 6  Herzeleid (Xxv Anniversary Edition – Remastered) 6HlDm9fzYxk4vkkr3YEvSv
    ## 7  Herzeleid (Xxv Anniversary Edition – Remastered) 6HlDm9fzYxk4vkkr3YEvSv
    ## 8  Herzeleid (Xxv Anniversary Edition – Remastered) 6HlDm9fzYxk4vkkr3YEvSv
    ## 9  Herzeleid (Xxv Anniversary Edition – Remastered) 6HlDm9fzYxk4vkkr3YEvSv
    ## 10 Herzeleid (Xxv Anniversary Edition – Remastered) 6HlDm9fzYxk4vkkr3YEvSv
    ## 11 Herzeleid (Xxv Anniversary Edition – Remastered) 6HlDm9fzYxk4vkkr3YEvSv
    ## 12                                        Sehnsucht 0C9p8YMtbdOkcXPPlEmZvY
    ## 13                                        Sehnsucht 0C9p8YMtbdOkcXPPlEmZvY
    ## 14                                        Sehnsucht 0C9p8YMtbdOkcXPPlEmZvY
    ## 15                                        Sehnsucht 0C9p8YMtbdOkcXPPlEmZvY
    ## 16                                        Sehnsucht 0C9p8YMtbdOkcXPPlEmZvY
    ## 17                                        Sehnsucht 0C9p8YMtbdOkcXPPlEmZvY
    ## 18                                        Sehnsucht 0C9p8YMtbdOkcXPPlEmZvY
    ## 19                                        Sehnsucht 0C9p8YMtbdOkcXPPlEmZvY
    ## 20                                        Sehnsucht 0C9p8YMtbdOkcXPPlEmZvY
    ## 21                                        Sehnsucht 0C9p8YMtbdOkcXPPlEmZvY
    ## 22                                        Sehnsucht 0C9p8YMtbdOkcXPPlEmZvY
    ## 23                                           Mutter 1CtTTpKbHU8KbHRB4LmBbv
    ## 24                                           Mutter 1CtTTpKbHU8KbHRB4LmBbv
    ## 25                                           Mutter 1CtTTpKbHU8KbHRB4LmBbv
    ## 26                                           Mutter 1CtTTpKbHU8KbHRB4LmBbv
    ## 27                                           Mutter 1CtTTpKbHU8KbHRB4LmBbv
    ## 28                                           Mutter 1CtTTpKbHU8KbHRB4LmBbv
    ## 29                                           Mutter 1CtTTpKbHU8KbHRB4LmBbv
    ## 30                                           Mutter 1CtTTpKbHU8KbHRB4LmBbv
    ## 31                                           Mutter 1CtTTpKbHU8KbHRB4LmBbv
    ## 32                                           Mutter 1CtTTpKbHU8KbHRB4LmBbv
    ## 33                                           Mutter 1CtTTpKbHU8KbHRB4LmBbv
    ## 34                                     Reise, Reise 09qHS2BgOLqi3SMkbauxdJ
    ## 35                                     Reise, Reise 09qHS2BgOLqi3SMkbauxdJ
    ## 36                                     Reise, Reise 09qHS2BgOLqi3SMkbauxdJ
    ## 37                                     Reise, Reise 09qHS2BgOLqi3SMkbauxdJ
    ## 38                                     Reise, Reise 09qHS2BgOLqi3SMkbauxdJ
    ## 39                                     Reise, Reise 09qHS2BgOLqi3SMkbauxdJ
    ## 40                                     Reise, Reise 09qHS2BgOLqi3SMkbauxdJ
    ## 41                                     Reise, Reise 09qHS2BgOLqi3SMkbauxdJ
    ## 42                                     Reise, Reise 09qHS2BgOLqi3SMkbauxdJ
    ## 43                                     Reise, Reise 09qHS2BgOLqi3SMkbauxdJ
    ## 44                                     Reise, Reise 09qHS2BgOLqi3SMkbauxdJ
    ## 45                                         Rosenrot 3PrS5X3QNdDUlBIE4bz6J2
    ## 46                                         Rosenrot 3PrS5X3QNdDUlBIE4bz6J2
    ## 47                                         Rosenrot 3PrS5X3QNdDUlBIE4bz6J2
    ## 48                                         Rosenrot 3PrS5X3QNdDUlBIE4bz6J2
    ## 49                                         Rosenrot 3PrS5X3QNdDUlBIE4bz6J2
    ## 50                                         Rosenrot 3PrS5X3QNdDUlBIE4bz6J2
    ## 51                                         Rosenrot 3PrS5X3QNdDUlBIE4bz6J2
    ## 52                                         Rosenrot 3PrS5X3QNdDUlBIE4bz6J2
    ## 53                                         Rosenrot 3PrS5X3QNdDUlBIE4bz6J2
    ## 54                                         Rosenrot 3PrS5X3QNdDUlBIE4bz6J2
    ## 55                                         Rosenrot 3PrS5X3QNdDUlBIE4bz6J2
    ## 56                            Liebe Ist Für Alle Da 3pksjSyo1w4vAPOenAzM4Z
    ## 57                            Liebe Ist Für Alle Da 3pksjSyo1w4vAPOenAzM4Z
    ## 58                            Liebe Ist Für Alle Da 3pksjSyo1w4vAPOenAzM4Z
    ## 59                            Liebe Ist Für Alle Da 3pksjSyo1w4vAPOenAzM4Z
    ## 60                            Liebe Ist Für Alle Da 3pksjSyo1w4vAPOenAzM4Z
    ## 61                            Liebe Ist Für Alle Da 3pksjSyo1w4vAPOenAzM4Z
    ## 62                            Liebe Ist Für Alle Da 3pksjSyo1w4vAPOenAzM4Z
    ## 63                            Liebe Ist Für Alle Da 3pksjSyo1w4vAPOenAzM4Z
    ## 64                            Liebe Ist Für Alle Da 3pksjSyo1w4vAPOenAzM4Z
    ## 65                            Liebe Ist Für Alle Da 3pksjSyo1w4vAPOenAzM4Z
    ## 66                            Liebe Ist Für Alle Da 3pksjSyo1w4vAPOenAzM4Z
    ## 67                            Liebe Ist Für Alle Da 3pksjSyo1w4vAPOenAzM4Z
    ## 68                            Liebe Ist Für Alle Da 3pksjSyo1w4vAPOenAzM4Z
    ## 69                            Liebe Ist Für Alle Da 3pksjSyo1w4vAPOenAzM4Z
    ## 70                            Liebe Ist Für Alle Da 3pksjSyo1w4vAPOenAzM4Z
    ## 71                            Liebe Ist Für Alle Da 3pksjSyo1w4vAPOenAzM4Z
    ## 72                                        Rammstein 1LoyJQVHPLHE3fCCS8Juek
    ## 73                                        Rammstein 1LoyJQVHPLHE3fCCS8Juek
    ## 74                                        Rammstein 1LoyJQVHPLHE3fCCS8Juek
    ## 75                                        Rammstein 1LoyJQVHPLHE3fCCS8Juek
    ## 76                                        Rammstein 1LoyJQVHPLHE3fCCS8Juek
    ## 77                                        Rammstein 1LoyJQVHPLHE3fCCS8Juek
    ## 78                                        Rammstein 1LoyJQVHPLHE3fCCS8Juek
    ## 79                                        Rammstein 1LoyJQVHPLHE3fCCS8Juek
    ## 80                                        Rammstein 1LoyJQVHPLHE3fCCS8Juek
    ## 81                                        Rammstein 1LoyJQVHPLHE3fCCS8Juek
    ## 82                                        Rammstein 1LoyJQVHPLHE3fCCS8Juek
    ##    danceability energy key loudness mode speechiness acousticness
    ## 1         0.673  0.766   2   -7.342    1      0.0394     2.37e-05
    ## 2         0.661  0.699   9   -7.982    1      0.0369     7.07e-06
    ## 3         0.603  0.750  10   -9.282    1      0.0378     9.40e-06
    ## 4         0.563  0.906   9   -7.503    1      0.0734     1.64e-03
    ## 5         0.600  0.634   4  -10.699    1      0.0442     3.03e-03
    ## 6         0.676  0.747   3   -7.732    1      0.0308     1.11e-04
    ## 7         0.631  0.850   4   -6.814    1      0.0719     1.09e-04
    ## 8         0.651  0.867   6   -7.880    0      0.1030     8.57e-04
    ## 9         0.703  0.720  11   -6.565    1      0.0370     2.14e-04
    ## 10        0.786  0.635   4   -8.304    1      0.0736     2.44e-04
    ## 11        0.657  0.708  11   -7.690    1      0.0564     1.55e-03
    ## 12        0.600  0.954  11   -5.463    0      0.0555     8.60e-03
    ## 13        0.637  0.658   2   -5.775    1      0.0291     9.18e-03
    ## 14        0.591  0.942   7   -4.975    1      0.0576     4.29e-03
    ## 15        0.710  0.858   4   -5.395    1      0.0361     6.15e-03
    ## 16        0.640  0.922   9   -6.283    0      0.0385     3.69e-03
    ## 17        0.693  0.979   9   -5.101    0      0.0913     6.55e-03
    ## 18        0.603  0.912   4   -6.041    1      0.1040     9.79e-02
    ## 19        0.581  0.589   5   -6.019    1      0.0346     7.81e-02
    ## 20        0.642  0.859   0   -5.422    1      0.1050     5.41e-03
    ## 21        0.816  0.968   9   -5.567    1      0.0542     1.30e-01
    ## 22        0.608  0.982  10   -4.777    1      0.0556     7.03e-03
    ## 23        0.501  0.760   2   -5.802    0      0.0343     2.68e-03
    ## 24        0.624  0.904   4   -4.774    1      0.0479     1.12e-03
    ## 25        0.510  0.917   4   -4.806    0      0.0854     5.40e-03
    ## 26        0.648  0.896   2   -4.254    1      0.0410     1.38e-03
    ## 27        0.491  0.966   3   -3.974    1      0.0463     9.12e-05
    ## 28        0.491  0.616   4   -6.530    1      0.0349     1.52e-02
    ## 29        0.640  0.838   2   -3.555    0      0.0428     9.73e-04
    ## 30        0.467  0.968   2   -3.991    1      0.0877     9.16e-06
    ## 31        0.670  0.856   9   -4.566    1      0.0488     1.07e-03
    ## 32        0.608  0.953   2   -4.903    0      0.0377     4.32e-04
    ## 33        0.628  0.428   2   -6.915    1      0.0285     4.69e-02
    ## 34        0.475  0.816   6   -5.502    1      0.0644     2.70e-03
    ## 35        0.470  0.959  10   -4.338    1      0.0750     5.35e-03
    ## 36        0.522  0.907   0   -5.062    1      0.0345     9.12e-03
    ## 37        0.534  0.892   5   -4.877    1      0.0537     2.56e-04
    ## 38        0.798  0.585   0   -7.099    1      0.0315     2.95e-03
    ## 39        0.608  0.885   2   -4.296    0      0.0963     2.58e-03
    ## 40        0.493  0.963  11   -3.410    1      0.0638     2.25e-05
    ## 41        0.514  0.945   8   -4.833    1      0.1050     2.34e-03
    ## 42        0.582  0.559   8   -5.755    1      0.0320     1.81e-04
    ## 43        0.424  0.590   2   -5.531    0      0.0294     3.89e-03
    ## 44        0.640  0.529   5   -7.180    0      0.0275     5.30e-03
    ## 45        0.527  0.947   4   -4.205    0      0.0574     3.93e-04
    ## 46        0.448  0.878   8   -5.288    1      0.0727     7.03e-04
    ## 47        0.591  0.629   8   -5.584    1      0.0273     1.44e-03
    ## 48        0.540  0.730   2   -5.807    0      0.0385     4.29e-03
    ## 49        0.464  0.779   7   -4.717    0      0.0478     9.05e-03
    ## 50        0.586  0.656  11   -5.615    0      0.0233     1.68e-02
    ## 51        0.412  0.775   9   -5.457    1      0.0417     7.88e-05
    ## 52        0.560  0.710   4   -5.113    1      0.0388     1.49e-04
    ## 53        0.496  0.934   5   -5.305    1      0.1450     7.84e-05
    ## 54        0.409  0.525   0   -7.745    0      0.0411     6.50e-03
    ## 55        0.442  0.190   5  -19.239    1      0.0335     5.35e-01
    ## 56        0.507  0.944   0   -4.350    0      0.0577     4.86e-04
    ## 57        0.295  0.878   2   -5.034    1      0.0541     1.12e-03
    ## 58        0.578  0.969   9   -3.894    1      0.0317     1.13e-03
    ## 59        0.627  0.842   9   -4.215    1      0.0375     8.32e-04
    ## 60        0.352  0.905   9   -3.792    1      0.0836     8.95e-04
    ## 61        0.553  0.816   6   -5.149    0      0.0305     1.75e-02
    ## 62        0.565  0.959   6   -4.745    0      0.1020     1.57e-03
    ## 63        0.573  0.773   7   -4.629    1      0.0442     1.58e-03
    ## 64        0.394  0.959   6   -3.520    1      0.0768     2.08e-04
    ## 65        0.356  0.852   1   -5.362    1      0.0360     2.97e-03
    ## 66        0.344  0.362   7  -10.677    1      0.0492     9.01e-01
    ## 67        0.538  0.804   2   -3.788    1      0.0295     6.68e-04
    ## 68        0.481  0.586   5   -4.963    1      0.0331     9.20e-04
    ## 69        0.636  0.886   0   -4.826    1      0.0424     1.11e-03
    ## 70        0.246  0.340   7  -10.255    1      0.0315     5.56e-01
    ## 71        0.471  0.387   4   -9.401    0      0.0375     3.27e-01
    ## 72        0.521  0.895   7   -5.242    1      0.0442     5.54e-05
    ## 73        0.652  0.894   0   -5.145    0      0.0409     4.04e-03
    ## 74        0.545  0.942   0   -4.283    0      0.0417     1.08e-03
    ## 75        0.681  0.912   0   -4.159    1      0.0912     7.57e-04
    ## 76        0.608  0.930   8   -3.970    1      0.0604     5.61e-04
    ## 77        0.581  0.822  11   -7.341    1      0.0574     2.29e-02
    ## 78        0.571  0.665   2   -4.953    1      0.0440     1.00e-03
    ## 79        0.347  0.272   0  -11.841    0      0.0375     8.13e-01
    ## 80        0.351  0.833   9   -5.109    1      0.0508     1.12e-02
    ## 81        0.612  0.960   9   -4.547    1      0.0910     2.62e-03
    ## 82        0.419  0.706  10   -5.936    1      0.0380     1.71e-02
    ##    instrumentalness liveness valence   tempo duration_ms album_release_year
    ## 1          6.73e-01   0.6990  0.6410 129.928      315850               1995
    ## 2          6.66e-01   0.1770  0.6290 130.144      250558               1995
    ## 3          8.39e-02   0.0667  0.6110 132.107      215998               1995
    ## 4          1.79e-02   0.0722  0.7100 169.967      231449               1995
    ## 5          1.92e-02   0.1010  0.2460 139.932      287857               1995
    ## 6          2.03e-01   0.3560  0.7970 111.850      291660               1995
    ## 7          5.57e-04   0.0812  0.6340 129.992      344227               1995
    ## 8          2.97e-04   0.2020  0.4790  94.783      284648               1995
    ## 9          6.83e-01   0.3080  0.7340 100.940      224705               1995
    ## 10         1.43e-01   0.0797  0.6870 133.604      262798               1995
    ## 11         7.63e-01   0.2150  0.2010 113.939      267743               1995
    ## 12         6.24e-01   0.0775  0.6650 140.125      244426               1997
    ## 13         2.29e-03   0.2950  0.3280  96.072      264266               1997
    ## 14         5.58e-02   0.1800  0.4560  95.084      226066               1997
    ## 15         1.66e-02   0.0400  0.8510 103.024      216866               1997
    ## 16         5.50e-02   0.3320  0.7160 125.105      234226               1997
    ## 17         2.91e-01   0.0265  0.6530 149.099      201533               1997
    ## 18         1.58e-01   0.0809  0.2780 156.169      285200               1997
    ## 19         0.00e+00   0.3480  0.2220 118.145      262053               1997
    ## 20         1.25e-01   0.0335  0.6160  84.981      262706               1997
    ## 21         6.92e-03   0.2180  0.8310 130.085      215866               1997
    ## 22         1.09e-03   0.2760  0.3790 106.060      210360               1997
    ## 23         6.16e-02   0.1520  0.1380  83.046      279893               2001
    ## 24         2.31e-02   0.6040  0.6490 130.153      216733               2001
    ## 25         1.33e-04   0.3960  0.3130  75.098      272440               2001
    ## 26         8.19e-04   0.0879  0.4680 128.115      217106               2001
    ## 27         3.46e-01   0.1530  0.2230  95.141      188600               2001
    ## 28         4.50e-04   0.1250  0.0824 116.145      268600               2001
    ## 29         4.58e-03   0.3070  0.6570 127.075      286173               2001
    ## 30         7.57e-01   0.3310  0.3540 160.170      257493               2001
    ## 31         1.31e-01   0.1640  0.7670 122.153      189973               2001
    ## 32         5.47e-02   0.0937  0.3770 115.160      228493               2001
    ## 33         1.20e-04   0.2110  0.1960 102.104      294600               2001
    ## 34         3.66e-01   0.3320  0.2480 130.065      251146               2004
    ## 35         1.01e-02   0.1410  0.1990  88.000      272413               2004
    ## 36         5.06e-06   0.1950  0.5000 156.034      338493               2004
    ## 37         1.37e-02   0.2320  0.2800 137.941      222933               2004
    ## 38         1.62e-01   0.1060  0.6490 124.027      263520               2004
    ## 39         1.33e-05   0.3920  0.5180 125.023      226760               2004
    ## 40         6.67e-01   0.3050  0.3050 147.992      256306               2004
    ## 41         1.58e-04   0.0638  0.4090 104.971      239866               2004
    ## 42         2.25e-01   0.4680  0.1180 120.002      232413               2004
    ## 43         4.32e-04   0.1140  0.2040 136.033      271173               2004
    ## 44         9.19e-02   0.1200  0.2590 104.023      290906               2004
    ## 45         3.72e-03   0.0750  0.2060 142.008      226000               2005
    ## 46         1.93e-01   0.0630  0.5790 166.105      230560               2005
    ## 47         4.31e-01   0.0705  0.1830 114.072      234706               2005
    ## 48         1.25e-02   0.1630  0.1680 116.099      324666               2005
    ## 49         3.60e-01   0.1260  0.1550 158.097      235360               2005
    ## 50         1.72e-04   0.1670  0.2890  88.004      245266               2005
    ## 51         1.37e-01   0.3200  0.1230 165.921      328933               2005
    ## 52         1.67e-03   0.0852  0.1980 107.988      283880               2005
    ## 53         3.44e-01   0.1150  0.2680 159.931      235626               2005
    ## 54         9.77e-04   0.1260  0.2230 160.033      312893               2005
    ## 55         4.38e-01   0.0928  0.0366 122.316      223480               2005
    ## 56         5.21e-01   0.0733  0.0689 104.967      318666               2009
    ## 57         1.91e-01   0.1130  0.2950 165.083      301853               2009
    ## 58         2.34e-02   0.0579  0.6840 104.999      212800               2009
    ## 59         8.98e-01   0.1150  0.3090 134.980      224906               2009
    ## 60         2.18e-01   0.1810  0.3270 141.016      254013               2009
    ## 61         1.48e-01   0.1270  0.4090 128.008      284920               2009
    ## 62         6.07e-03   0.1190  0.1790 109.951      232120               2009
    ## 63         9.57e-02   0.0734  0.5410 134.977      239080               2009
    ## 64         9.16e-02   0.1140  0.4290 180.049      205440               2009
    ## 65         5.55e-01   0.0794  0.4690 100.080      248253               2009
    ## 66         7.35e-02   0.1240  0.1550 128.723      239000               2009
    ## 67         5.92e-01   0.1150  0.6970 156.060      273026               2009
    ## 68         5.37e-01   0.1090  0.0891 120.004      317960               2009
    ## 69         8.19e-01   0.4540  0.2130 119.053      260933               2009
    ## 70         7.38e-02   0.1170  0.0792  82.421      245173               2009
    ## 71         4.07e-01   0.1680  0.0517 110.186      235573               2009
    ## 72         3.49e-01   0.0985  0.2370 120.117      322339               2019
    ## 73         3.81e-03   0.1240  0.7600 132.030      277397               2019
    ## 74         2.46e-02   0.0585  0.3650 165.066      255845               2019
    ## 75         1.36e-03   0.3590  0.4790 125.031      231603               2019
    ## 76         1.75e-03   0.1560  0.4320 123.018      236529               2019
    ## 77         0.00e+00   0.1260  0.3000 116.929      273706               2019
    ## 78         6.21e-02   0.0874  0.5820 172.005      269045               2019
    ## 79         2.37e-05   0.1020  0.1220 140.506      154298               2019
    ## 80         2.92e-04   0.1660  0.3030 202.036      260915               2019
    ## 81         7.93e-05   0.0846  0.3020 141.042      251658               2019
    ## 82         2.44e-02   0.0998  0.1430 171.997      251634               2019
    ##    track_number
    ## 1             1
    ## 2             2
    ## 3             3
    ## 4             4
    ## 5             5
    ## 6             6
    ## 7             7
    ## 8             8
    ## 9             9
    ## 10           10
    ## 11           11
    ## 12            1
    ## 13            2
    ## 14            3
    ## 15            4
    ## 16            5
    ## 17            6
    ## 18            7
    ## 19            8
    ## 20            9
    ## 21           10
    ## 22           11
    ## 23            1
    ## 24            2
    ## 25            3
    ## 26            4
    ## 27            5
    ## 28            6
    ## 29            7
    ## 30            8
    ## 31            9
    ## 32           10
    ## 33           11
    ## 34            1
    ## 35            2
    ## 36            3
    ## 37            4
    ## 38            5
    ## 39            6
    ## 40            7
    ## 41            8
    ## 42            9
    ## 43           10
    ## 44           11
    ## 45            1
    ## 46            2
    ## 47            3
    ## 48            4
    ## 49            5
    ## 50            6
    ## 51            7
    ## 52            8
    ## 53            9
    ## 54           10
    ## 55           11
    ## 56            1
    ## 57            2
    ## 58            3
    ## 59            4
    ## 60            5
    ## 61            6
    ## 62            7
    ## 63            8
    ## 64            9
    ## 65           10
    ## 66           11
    ## 67           12
    ## 68           13
    ## 69           14
    ## 70           15
    ## 71           16
    ## 72            1
    ## 73            2
    ## 74            3
    ## 75            4
    ## 76            5
    ## 77            6
    ## 78            7
    ## 79            8
    ## 80            9
    ## 81           10
    ## 82           11

## Including Pl

``` describe
```
