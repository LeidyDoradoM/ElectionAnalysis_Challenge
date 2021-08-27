# Election Analysis 

In this week project, we are helping out the Election Board of Colorado to get a *Python script* that automatically tabulates the results for congressional election.  If the script works well, it could be used in another elections such as senatorial or local elections for another disctricts.

## Overview of Election Audit

Per initial request from the *Election Commission*, the election audit included five results such as total number of votes cast, a complete list of candidates who received votes, total votes for each candidate, the percentage of these votes and the winner of the election.  Now, we need to expand these results so the audit could be completed and also includes:

1. The vote turnout for each county
2. The percentage of votes from each county out of the total count.
3. The county with the highest turnout.

There are three primary voting methods: Mail-in ballots, punch cards and Direct Recording Electronic (DRE) counting. Votes cast by these ways are organized as a table in *csv* format, [election_results.csv](https://github.com/LeidyDoradoM/ElectionAnalysis_Challenge/tree/main/Resources ).  The data has three columns: **Ballot ID**, **County**, **Candidate**, and **369,711** rows that represent the each vote cast.

## Election-Audit Results

The Colorado Board of Election has listed the questions we need to answer from the data collected in the voting process.  The responses to these answers are given below and they are a copy of the terminal output that we got after running our *Python script*.  In addition, a txt file has been created, [election_analysis](https://github.com/LeidyDoradoM/ElectionAnalysis_Challenge/tree/main/analysis/election_analysis.txt ) to save all the information generated on this Election-audit.  

1. How many votes were cast in this congressional election?.

By inspection of the original data, we can infer that the total votes are equal to the number of rows in the file `election_results.csv`.  We can calculate this number in our script by iterating over the number of rows and counting the times that we go through the iterations.

```vb
Election Results
-------------------------
Total Votes: 369,711
-------------------------
```

2. Number of votes and the percentage of total votes for each county in the precinct. 

To find the number of votes cast by county, we need to identify first unique county names in the data and then count the number of rows whose value in the county column corresponds to each identified unique county.  From this process, there are three counties identified in the data : *Jefferson, Denver, Arapahoe*. Bellow it is the total number of votes for each county and their corresponding percentages.

```vb
County Votes:
Jefferson: 10.5% (38,855)

Denver: 82.8% (306,055)

Arapahoe: 6.7% (24,801)
```

3. Which county had the largest number of votes?.

From the results shown in the previous question, we can compare the total votes for each of the three counties and choose the county with the largest number. Bellow it is the result that is shown when the code is run.

```vb
----------------------------
Largest County Turnout: Denver
-----------------------------
```

4. Number of votes and the percentage of the total votes each candidate received.

Similarly to the process followed to calculate the number of votes per county, the candidates are identified and then the corresponding number of rows are counted. With the counted votes for each candidate, the percentage is easily calculated.

```vb
Charles Casper Stockham: 23.0% (85,213)

Diana DeGette: 73.8% (272,892)

Raymon Anthony Doane: 3.1% (11,606)
```
5. Which candidate won the election, what was their vote count, and what was their percentage of the total votes?

With all the votes counted for each candidate, the winner of the election can be determined by comparing the number of votes that each candidate received and choose the one with the largest number of votes. The values shown below are exactly the numbers given by the script.
```vb
-------------------------
Winner: Diana DeGette
Winning Vote Count: 272,892
Winning Percentage: 73.8%
-------------------------
```
The main part of the *Python* script that calculates the above results is shown below. The entire script besides to compute the results of the election-audit, prints them in the terminal at the same time the code is run and also saves the results in a text file.  The entire script can be found in [PyPoll_Challenge.py](https://github.com/LeidyDoradoM/ElectionAnalysis_Challenge/tree/main/PyPoll_Challenge.py )
```vb
# Read the csv and convert it into a list of dictionaries
with open(file_to_load) as election_data:
    reader = csv.reader(election_data)
    # Read the header
    header = next(reader)
    # For each row in the CSV file.
    for row in reader:
        # Add to the total vote count
        total_votes = total_votes + 1
        # Get the candidate name from each row.
        candidate_name = row[2]
        # 3: Extract the county name from each row.
        county_name = row[1]
        # If the candidate does not match any existing candidate add it to
        # the candidate list
        if candidate_name not in candidate_options:
            # Add the candidate name to the candidate list.
            candidate_options.append(candidate_name)
            # And begin tracking that candidate's voter count.
            candidate_votes[candidate_name] = 0
        # Add a vote to that candidate's count
        candidate_votes[candidate_name] += 1
        # 4a: Write an if statement that checks that the
        # county does not match any existing county in the county list.
        if county_name not in county_list:
            # 4b: Add the existing county to the list of counties.
            county_list.append(county_name)
            # 4c: Begin tracking the county's vote count.
            county_votes[county_name] = 0
        # 5: Add a vote to that county's vote count.
        county_votes[county_name] += 1
```

## Election-Audit Summary

This script was implemented to audit a congressional election at disctict level, however, it can be extended to audit any type of election, where votes can be cast from another electoral districts to get winner candidates at state level such as **Senate** or **Presidential** elections.  In these cases, we need that the script makes the counting of votes for the 8 congressional districts in which the Colorado state has been divided. Assuming we have access to the data from the another 7 congressional districts, we need to run this script 8 times and save the results for each congressional district.  Then, the winner candidate ( in the case of Presidential election), can be determined by considering the candidate with the largest number of votes among the 8 winner candidates for each electoral district.  In the Senate election, we need to elect two candidates, so among the 8 winner candidates, we need to choose the first two candidates with the two largest numbers of votes.

In order to get this extension in the code, we would need to authomate the process by adding a for loop that iterates through the 8 congressional districts and within this loop enclosed the code that we already have implemented.  Besides, we need to add the lines that save the results for each district in a dictionary, where the keys are the districts and the values are lists conatining the diverse results that we already have considered (winner of the district, counties with largest count of votes, list of candidates and their corresponding votes). Then, to determine the winner candidate(s) we access the dictionary by each key and keep from the value-list only the winner candidate information.  Once we have the access to each winner for each district we can choose the candidate with the largest number of votes.