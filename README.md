# Bike-Share-Analaysis-
A simple application on data analysis using numpy and pandas on Bike share dateset to answer a set of questions to understand the data further more.
import time
import pandas as pd
import numpy as np

CITY_DATA = { 'chicago': 'chicago.csv',
              'new york city': 'new_york_city.csv',
              'washington': 'washington.csv' }

def get_filters():
    """
    Asks user to specify a city, month, and day to analyze.

    Returns:
        (str) city - name of the city to analyze
        (str) month - name of the month to filter by, or "all" to apply no month filter
        (str) day - name of the day of week to filter by, or "all" to apply no day filter
    """
    print('Hello! Let\'s explore some US bikeshare data!')
    # TO DO: get user input for city (chicago, new york city, washington). HINT: Use a while loop to handle invalid inputs
    city = ""
    while city not in CITY_DATA.keys():
        city = input("Which city would you like to check its data Chicago or New York City or Washington (i.e new york city)").lower()
    
    
    choice = input("Would you like to filter the data by month, day, both or not at all? Type 'none' for no time filter ")
    
    if choice.lower() == "month":
        # TO DO: get user input for month (all, january, february, ... , june)
        months = ['all', 'january', 'february', 'march', 'april', 'may', 'june']
        month = ""
        day = 'all'
        while month not in months:
            print("Please Enter month from month (all, january, february, ... , june)")
            month = input().lower()
            
    elif choice.lower() == "day":
        # TO DO: get user input for day of week (all, monday, tuesday, ... sunday)
        days = ['all', 'monday', 'tuesday', 'wednesday', 'thursday', 'friday',  'sunday']
        day = ""
        month = 'all'
        while day not in days:
            print("Please Enter day from day (all, monday, tuesday, ... sunday)")
            day = input().lower()
            
    elif choice.lower() == "both":
        months = ['all', 'january', 'february', 'march', 'april', 'may', 'june']
        month = ""
        while month not in months:
            print("Please Enter month from month (all, january, february, ... , june)")
            month = input().lower()
            
        days = ['all', 'monday', 'tuesday', 'wednesday', 'thursday', 'friday',  'sunday']
        day = ""
        while day not in days:
            print("Please Enter day from day (all, monday, tuesday, ... sunday)")
            day = input().lower()
    else:
        month ='all'
        day = 'all'
        pass

    print('-'*40)
    return city, month, day


def load_data(city, month='all', day='all'):
    """
    Loads data for the specified city and filters by month and day if applicable.

    Args:
        (str) city - name of the city to analyze
        (str) month - name of the month to filter by, or "all" to apply no month filter
        (str) day - name of the day of week to filter by, or "all" to apply no day filter
    Returns:
        df - Pandas DataFrame containing city data filtered by month and day
    """
    df = pd.DataFrame(pd.read_csv(CITY_DATA[city]))
    df['Start Time'] = pd.to_datetime(df['Start Time'])
    
    view_data = input("Would you like to view 5 rows of individual trip data? Enter yes or no?")
    start_loc = 0
    while (view_data.lower() == 'yes'):
        print(df.iloc[start_loc:(start_loc+5)])
        start_loc += 5
        view_data = input("Do you wish to continue?: Enter yes or no ")
    
    df['day'] = df['Start Time'].dt.day_name()
    df['month'] = df['Start Time'].dt.month_name()
    df['hour'] = df['Start Time'].dt.hour

    if month != 'all':
        df = df[(df['month']==month.title())]
        
    if day != 'all':
        df = df[((df['day']== day.title()))]

    return df


def time_stats(df):
    """Displays statistics on the most frequent times of travel."""

    print('\nCalculating The Most Frequent Times of Travel...\n')
    start_time = time.time()

    # TO DO: display the most common month
    most_common_month = df['month'].mode()[0]
    print('Most Common Month:', most_common_month)

    # TO DO: display the most common day of week
    most_common_day = df['day'].mode()[0]
    print('Most Common Day:', most_common_day)

    # TO DO: display the most common start hour
    most_common_hour = df['hour'].mode()[0]
    print('Most Common Start Hour:', most_common_hour)


    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def station_stats(df):
    """Displays statistics on the most popular stations and trip."""

    print('\nCalculating The Most Popular Stations and Trip...\n')
    start_time = time.time()

    # TO DO: display most commonly used start station
    most_common_sstation = df['Start Station'].mode()[0]
    print('Most Common Start Station:', most_common_sstation)

    # TO DO: display most commonly used end station
    most_common_estation = df['End Station'].mode()[0]
    print('Most Common End Station:', most_common_estation)

    # TO DO: display most frequent combination of start station and end station trip
    most_common_combination = df.groupby(['Start Station','End Station']).size().nlargest(1)
    print('Most Common End Station:', most_common_combination)

    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def trip_duration_stats(df):
    """Displays statistics on the total and average trip duration."""

    print('\nCalculating Trip Duration...\n')
    start_time = time.time()

    # TO DO: display total travel time
    total_travel_time = df['Trip Duration'].sum()
    print('Total Travel Time: ',total_travel_time,'minutes')
    # TO DO: display mean travel time
    avg_travel_time = df['Trip Duration'].mean()
    print('average Travel Time: ',avg_travel_time,'minutes')   

    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def user_stats(df):
    """Displays statistics on bikeshare users."""

    print('\nCalculating User Stats...\n')
    start_time = time.time()

    # TO DO: Display counts of user types
    user_types_count = df['User Type'].value_counts()
    print("The counts of each user type are: ", user_types_count)

    # TO DO: Display counts of gender
    if 'Gender' in df.columns :
        gender_count = df['Gender'].value_counts()
        print("The counts of each user type are: ", gender_count)
    else:
        pass

    # TO DO: Display earliest, most recent, and most common year of birth
    if 'Birth Year' in df.columns :
        most_common_year = df['Birth Year'].value_counts().nlargest(1)
        print("The most common year of birth: ",most_common_year)
        most_recent_year = df['Birth Year'].max()
        print("The most recent year of birth: ",most_recent_year)
        most_earliest_year = df['Birth Year'].min()
        print("The most recent year of birth: ",most_earliest_year)
    else:
        pass


    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def main():
    while True:
        city, month, day = get_filters()
        df = load_data(city, month, day)

        time_stats(df)
        station_stats(df)
        trip_duration_stats(df)
        user_stats(df)

        restart = input('\nWould you like to restart? Enter yes or no.\n')
        if restart.lower() != 'yes':
            break


if __name__ == "__main__":
	main()

----------------------------------------- *** Results *** ------------------------------------------------
Hello! Let's explore some US bikeshare data!
Which city would you like to check its data Chicago or New York City or Washington (i.e new york city)chicago
Would you like to filter the data by month, day, both or not at all? Type 'none' for no time filter none
----------------------------------------
Would you like to view 5 rows of individual trip data? Enter yes or no?yes
   Unnamed: 0          Start Time             End Time  Trip Duration  \
0     1423854 2017-06-23 15:09:32  2017-06-23 15:14:53            321   
1      955915 2017-05-25 18:19:03  2017-05-25 18:45:53           1610   
2        9031 2017-01-04 08:27:49  2017-01-04 08:34:45            416   
3      304487 2017-03-06 13:49:38  2017-03-06 13:55:28            350   
4       45207 2017-01-17 14:53:07  2017-01-17 15:02:01            534   

                   Start Station                   End Station   User Type  \
0           Wood St & Hubbard St       Damen Ave & Chicago Ave  Subscriber   
1            Theater on the Lake  Sheffield Ave & Waveland Ave  Subscriber   
2             May St & Taylor St           Wood St & Taylor St  Subscriber   
3  Christiana Ave & Lawrence Ave  St. Louis Ave & Balmoral Ave  Subscriber   
4         Clark St & Randolph St  Desplaines St & Jackson Blvd  Subscriber   

   Gender  Birth Year  
0    Male      1992.0  
1  Female      1992.0  
2    Male      1981.0  
3    Male      1986.0  
4    Male      1975.0  
Do you wish to continue?: Enter yes or no yes
   Unnamed: 0          Start Time             End Time  Trip Duration  \
5     1473887 2017-06-26 09:01:20  2017-06-26 09:11:06            586   
6      961916 2017-05-26 09:41:44  2017-05-26 09:46:25            281   
7       65924 2017-01-21 14:28:38  2017-01-21 14:40:41            723   
8      606841 2017-04-20 16:08:51  2017-04-20 16:20:20            689   
9      135470 2017-02-06 18:00:47  2017-02-06 18:09:00            493   

                  Start Station                    End Station   User Type  \
5  Clinton St & Washington Blvd           Canal St & Taylor St  Subscriber   
6         Ashland Ave & Lake St           Wood St & Hubbard St  Subscriber   
7    Larrabee St & Kingsbury St     Larrabee St & Armitage Ave    Customer   
8        Sedgwick St & Huron St  Halsted St & Blackhawk St (*)  Subscriber   
9  Stetson Ave & South Water St   Clinton St & Washington Blvd  Subscriber   

   Gender  Birth Year  
5    Male      1990.0  
6  Female      1983.0  
7     NaN         NaN  
8    Male      1984.0  
9    Male      1979.0  
Do you wish to continue?: Enter yes or no no

Calculating The Most Frequent Times of Travel...

Most Common Month: June
Most Common Day: Tuesday
Most Common Start Hour: 17

This took 0.041884422302246094 seconds.
----------------------------------------

Calculating The Most Popular Stations and Trip...

Most Common Start Station: Streeter Dr & Grand Ave
Most Common End Station: Streeter Dr & Grand Ave
Most Common End Station: Start Station              End Station            
Lake Shore Dr & Monroe St  Streeter Dr & Grand Ave    854
dtype: int64

This took 0.10771608352661133 seconds.
----------------------------------------

Calculating Trip Duration...

Total Travel Time:  280871787 minutes
average Travel Time:  936.23929 minutes

This took 0.0009772777557373047 seconds.
----------------------------------------

Calculating User Stats...

The counts of each user type are:  Subscriber    238889
Customer       61110
Dependent          1
Name: User Type, dtype: int64
The counts of each user type are:  Male      181190
Female     57758
Name: Gender, dtype: int64
The most common year of birth:  1989.0    14666
Name: Birth Year, dtype: int64
The most recent year of birth:  2016.0
The most recent year of birth:  1899.0

This took 0.046892404556274414 seconds.
