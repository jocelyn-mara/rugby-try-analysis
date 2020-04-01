# rugby-try-analysis
The goal of this repo is to provide an example of hosting a data analysis project on GitHub for UC students as part of the Applied Data Analysis in Sport unit. 

This repo contains the following:

- an `exploring-rugby-data.Rmd` file containing the R code for analysing the data
- `exploring-rugby-data.html` and `exploring-rugby-data.pdf` files which show the rendered reports, from the above R Markdown document
- a `data` directory containing the `2017_super-rugby_try-source-data.csv` data file.

This data consists of tries that were scored during the 2017 Super Rugby competition (observations/rows). Here is a description of the variables:

1. `try_no`: a unique identification number given to each try
2. `round_no`: an identification number to distinguish the round number the try was scored in
3. `attacking_team`: the try-scoring team
4. `defending_team`: the opposition team who conceded the try
5. `attacking_rank`: the final league ranking at the end of the season of the try-scoring team
6. `defending_rank`: the final league ranking at the end of the season of the opposition team
7. `attacking_conference`: the conference group of the try-scoring team
8. `defending_conference`: the conference group of the opposition team
9. `game_time`: the game time in minutes when the try was scored
10. `try_source`: the initial source of possession for the attacking team preceding the try
11. `final_source`: the event that directly preceded the try and resulted in the try being scored
12. `phases`: the total number of phases between gaining possession, and the try being scored (a phase is from one ruck to the next ruck)
13. `time_from_source`: the time taken from gaining possession to scoring the try, in seconds
14. `possession_zone`: the zone on the field the attacking team gained possession of the ball before scoring the try (A = attacking 22m line to try-line, B = halfway to attacking 22m line, C = defensive 22m line to halfway, D = )
15. `offloads`: the number of offloads from gaining possession to the try being scored
15. `passes`: the number of passes from gaining possession to the try being scored
16. `total_passes`: the number of offloads plus passes

This data was collected by a former University of Canberra student, Molly Coughlan, as part of a project that identified playing patterns that led to tries in super rugby (Coughlan, Mountifield, Sharpe & Mara, 2019. How they scored the tries: applying cluster analysis to identify playing patterns that lead to tries in super rugby. IJPAS.)
