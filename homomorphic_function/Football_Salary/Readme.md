# Dataset of football salary
[found on Kaggle](https://www.kaggle.com/datasets/ultimus/football-salaries-dataset/data)

## Use case

A football club wants to ensure that the salaries they offer to their players are competitive with respect to other clubs. To achieve this, they have hired our services to analyze the average salary of players using homomorphic encryption.

All data sended to us is already encrypted.
## Task
Execute basics statistical methods on the data:
1. Average salary 
2. Average salary grouped by player nationality
3. Standard deviation of salaries
4. median salary

## Data 

### Dataset Columns Description
- Row    :40791
- Column : 16
- League(New name)= Division(old name)

| Column Name        | Description                                      | Format                                  |
|------------------  |--------------------------------------------------|-----------------------------------------|
| `Name`             | Full name of the player along with nationality   | "{Full name} - {Nationality}"           |
| `Club`             | Current club and division of the player          | "{Club} - {Division}"                   |
| `Division`         | Division where the player's club competes        | "{Division name}"                       |
| `Based`            | Country and division details                     | "{Country} ({Division abbreviation})"   |
| `Nat`              | Nationality abbreviation (ISO 3166-1 alpha-3)    | "ISO 3166-1 alpha-3 code"               |
| `EU National`      | Indicates if the player is an EU national        | "Yes/No"                                |
| `Caps`             | Number of international matches played           | Uint                                    |
| `AT Apps`          | Appearances in matches                           | Uint                                    |
| `Position`         | Player's position on the field                   | "{Position} ({Position Details}),..."   |
| `Age`              | Age of the player                                | Uint                                    |
| `CR`               | Current reputation (scale of 0-10,000)           | Float                                   |
| `Begins`           | Date when the player started with the current club | "{dd/mm/yyyy}"                         |
| `Expires`          | Contract expiration date                         | "{dd/mm/yyyy}"                          |
| `Last Club`        | Previous club of the player                      | "{Club}"                                |
| `Last Trans. Fee`  | Transfer fee for the last club transition        | "€{Amount}M / - / €0"                   |
| `Salary`           | Annual salary of the player                      | "€{#,###,###} p/a"                      |

### Overview
- **Unique Players**:  only 10 player names listed.
- **Clubs & Divisions**:  5 clubs and 792 divisions,.
- **Age**: min:17 max:45 ,  average: ~25 years.
- **Contract Dates**: between 2023 and 2024.


