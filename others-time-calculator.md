## [solution1](https://repl.it/@kelvinho1234/boilerplate-time-calculator#time_calculator.py)

```python
def add_time(start, duration, day=None):
  
  start_hr = int(start[:-2].split(":")[0])
  start_min = int(start[:-2].split(":")[1])
  duration_hr = int(duration.split(":")[0])
  duration_min = int(duration.split(":")[1])
  period = start[-2:]
  weekday = ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"]
  
  if start_hr == 12 and period == "AM":
    start_hr = 0 
  if start_hr == 12 and period == "PM":
    start_hr = 0 

  if start_min + duration_min > 60 :
    end_min = start_min + duration_min - 60
    if period == "PM" : 
      end_hr = start_hr + duration_hr + 1 + 12
    else: 
      end_hr = start_hr + duration_hr + 1
  else:
    end_min = start_min + duration_min
    if period == "PM" : 
      end_hr = start_hr + duration_hr + 12
    else:
      end_hr = start_hr + duration_hr

  if end_hr//24 < 1:
    day_count = ""
  elif end_hr//24 == 1:
    day_count = " (next day)"
  else:
    day_count = " (" + str(end_hr//24) + " days later)"

  if end_hr - ((end_hr//24)*24) < 12:
    if end_hr - ((end_hr//24)*24) == 0:
      new_hr = end_hr - ((end_hr//24)*24) + 12 
    else:
      new_hr = end_hr - ((end_hr//24)*24) 
    new_period = " AM"
  else:
    if end_hr - ((end_hr//24)*24) == 12:
      new_hr = end_hr - ((end_hr//24)*24)
    else:
      new_hr = end_hr - ((end_hr//24)*24) - 12
    new_period = " PM"

  if day != None:
    week_day_count = int(weekday.index(day.lower().capitalize())) + end_hr//24
    new_weekday = ", " + weekday[week_day_count % 7] 
  else:
    new_weekday = ""

  new_time = str(new_hr) + ":" + str(end_min).zfill(2) + new_period  + new_weekday + day_count

  return new_time
``` 


## [solution2](https://repl.it/@robertue1/fcc-time-calculator#time_calculator.py)

```python
  def add_time(start, duration, initialday='None'):

#Getting the initial time
#First, let's get the hour
  start_h = int(start.split(':')[0])
  #Second, let's get the initial minute
  start_m = int(start.split(':')[1].split()[0])
  #Last, but not least, finding out if we are talking about time ante or post meridiem
  meridiem = start.split(':')[1].split()[1]

  #Now, let's get the duration
  addedh = int(duration.split(':')[0])
  addedm = int(duration.split(':')[1].split()[0])

  #let's start by adding the minutes. The range of the minutes will be between 0 and 60. 
  newminute = start_m + addedm
  #if we go over 60, we will add an hour to the addedh. 
  if newminute > 59:
      newminute = newminute - 60 
      addedh = addedh + 1
      
  #Let's make sure that we transform anything bigger that 24 h into something inside the range of 0 and 24. Also, let's start taking into consideration how many days are we gonna be shifting in time. 
  ndays=0
  while addedh >= 24:
      ndays = ndays+1
      addedh = addedh - 24
      

  #Let's handle with the result of the newh after modifying the addedh, for the first case
  #When we add 12 hours, we will be just changing the AM or PM for the opposite. 
  if addedh==12:
      newh=start_h
      #If we add 12 or more hours and we start let's say at noon, we will jump to the next day. 
      if meridiem == "PM":
          print("Heeeeello")
          ndays = ndays + 1
          meridiem = "AM"
      else:
          meridiem = "PM"
  #When we add more than 12 hours, the change in AM or PM depends in the initial hour too.         
  if addedh>12:
      addedh = addedh-12
      newh=start_h + addedh
      if newh<12:
          if meridiem == "PM":
              ndays = ndays + 1
              meridiem = "AM"
          else:
              meridiem = "PM"
      
      if newh==12:
          ndays = ndays + 1
      
      if newh>12:
          newh=newh-12
          if meridiem == "PM":
              ndays = ndays + 1
              meridiem = "AM"
          else:
              meridiem = "PM"
      
  #Now, let's do the case when we add less than 12 hours
  if addedh<12:
  #A little something that I add because it was giving me trouble when the initial time was 12. 
      if start_h==12:
          start_h=0
          newh=start_h+addedh
      else:
        newh=start_h+addedh
      if newh == 12:
          if meridiem == "PM":
              ndays = ndays + 1
              meridiem = "AM"
          else:
              meridiem = "PM"
          
      if newh>12:
          newh = newh-12
          if meridiem == "PM":
              ndays = ndays + 1
              meridiem = "AM"
          else:
              meridiem = "PM"

   #I create a list with the week of the days.    
  daysoftheweek = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday","Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"]

  stringh = str(newh)

  if newminute<10:
    stringm = "0" + str(newminute)
  else:
    stringm= str(newminute)

  auxndays = ndays
  if initialday != "None":
  #Creating an auxiliar parameter
    y = initialday.lower().capitalize()
  #With this, I can look for the index of the day of interest in my daysoftheweek list
    indexa = daysoftheweek.index(y) 
    

    if ndays==0:
      new_time = stringh + ":" + stringm + " " + meridiem + ", " + daysoftheweek[indexa]
    elif ndays<=7:
      newindex = indexa + ndays
      if ndays == 1:
        new_time = stringh + ":" + stringm + " " + meridiem +", " + daysoftheweek[newindex] + " (next day)"
      else:
        new_time = stringh + ":" + stringm + " " + meridiem +", " + daysoftheweek[newindex] + " (" +str(auxndays)+ " days later)"
    elif ndays>7:
      while ndays > 7:
        ndays = ndays -7
      newindex = indexa + ndays
      new_time = stringh + ":" + stringm + " " + meridiem +", " + daysoftheweek[newindex] + " (" +str(auxndays)+ " days later)"
  else:
    if ndays==0:
      new_time = stringh + ":" + stringm + " " + meridiem
    elif ndays <=7:
      if ndays == 1:
        new_time = stringh + ":" + stringm + " " + meridiem +  " (next day)"
      else:
        new_time = stringh + ":" + stringm + " " + meridiem + " (" +str(auxndays)+ " days later)"
    elif ndays>7:
      while ndays > 7:
        ndays = ndays -7
      new_time = stringh + ":" + stringm + " " + meridiem + " (" +str(auxndays)+ " days later)"

  return new_time
  ```
  
  ## [solution3](https://repl.it/@ManshulArora/time-calculator#time_calculator.py)
  
```python
  def add_time(start, duration, day_name = None):

  #Splitting the time strings 

  add_hours = int(duration.split(':')[0])
  add_minutes = int(duration.split(':')[1])

  initial_hour = int(start.split(':')[0])
  initial_minute = int((start.split(':')[1]).split(' ')[0])
  am_pm = (start.split(':')[1]).split(' ')[1]


  # Summing the initial time to total number of hours 
  if am_pm == 'PM':
    total_initial_hour = 12+ initial_hour
  else:
    total_initial_hour = initial_hour

  new_total_hour = total_initial_hour+add_hours
  new_minute = initial_minute+ add_minutes


  #New Hour and New Time 

  if new_minute >= 60:
    new_minute -= 60
    new_total_hour += 1 
  else:
    new_minute = new_minute
    new_total_hour = new_total_hour

  if new_total_hour >= 24:

    days = new_total_hour//24 #Calculating extra days 
    new_total_hour = new_total_hour%24
    
    if new_total_hour == 0 :
      new_am_pm = 'AM'
      new_total_hour = 12 
    
    elif new_total_hour >12:
      new_am_pm = 'PM'
      new_total_hour -= 12

    else:
      new_am_pm = 'AM'
      new_total_hour = new_total_hour
  
  else:
    
    days = 0 

    if new_total_hour >=12:
      new_am_pm = 'PM'
      new_total_hour = new_total_hour-12
      if new_total_hour == 0 :
        new_total_hour = 12
    else:
      new_am_pm = 'AM'
      new_total_hour = new_total_hour

  
  if days == 0 :
    return_day = f"" 
  elif days <= 1:
    return_day = f'(next day)'
  elif days>1:
    return_day = f'({days} days later)'


  #Coding if the day is given 

  day_list = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday','Friday','Saturday']

  if day_name != None: 
    if day_name.capitalize() in day_list:
      initial_day_index = day_list.index(day_name.capitalize())
      new_day_index = initial_day_index + days
      if new_day_index > 6:
        new_day_index = ((new_day_index+1)%7)-1
        new_day = day_list[new_day_index]
      else:
        new_day = day_list[new_day_index]
  
  if days == 0:
    if day_name != None: 
      final_result = f"{new_total_hour}:{new_minute:02} {new_am_pm}, {new_day}"
    else:
      final_result = f"{new_total_hour}:{new_minute:02} {new_am_pm}"

  else:
    if day_name != None:
      final_result = f"{new_total_hour}:{new_minute:02} {new_am_pm}, {new_day} {return_day}"
    
    else: 
      final_result = f"{new_total_hour}:{new_minute:02} {new_am_pm} {return_day}"
 
  return final_result
```
  
## [solution4](https://github.com/ShreyasSubhedar/fcc-time-calculator/blob/master/time_calculator.py)
  
```python
  def add_time(start, duration, day=None):
    # Maintaining Days in a week
    day_map = {
        "Saturday": 0,
        "Sunday": 1,
        "Monday": 2,
        "Tuesday": 3,
        "Wednesday": 4,
        "Thursday": 5,
        "Friday": 6
    }
    # Getting useful data from start string
    timely, midday = start.split()
    hour, minutes = timely.split(':')
    hour = int(hour)
    minutes = int(minutes)

    # Making the clock into 24 hour format
    if midday == "PM":
        hour += 12

    # Getting data from duration
    duration_hour, duration_minutes = duration.split(':')
    duration_hour = int(duration_hour)
    duration_minutes = int(duration_minutes)

    # Calculating total hours, minutes
    total_minutes = minutes + duration_minutes
    ans_minutes = total_minutes % 60
    extra_hours = total_minutes // 60
    total_hour = hour + duration_hour + extra_hours

    # final hours as per 12 Hour clock
    ans_hour = (total_hour % 24) % 12

    # Edge case
    if ans_hour == 0:
        ans_hour = 12
    ans_hour = str(ans_hour)

    # total days 24 hr 1 day
    total_day = (total_hour // 24)

    # deciding mid day (AM/PM)
    ans_midday = ""
    if (total_hour % 24) <= 11:
        ans_midday = "AM"
    else:
        ans_midday = "PM"

    # Handling single digit minutes case
    if ans_minutes <= 9:
        ans_minutes = '0' + str(ans_minutes)
    else:
        ans_minutes = str(ans_minutes)
    # returning logic
    time_stamp = ans_hour + ":" + ans_minutes + ' ' + ans_midday
    if day == None:
        if total_day == 0:
            return time_stamp
        if total_day == 1:
            return time_stamp + ' (next day)'
        return time_stamp + ' (' + str(total_day) + ' days later)'
    else:
        ans_day = (day_map[day.lower().capitalize()] + total_day) % 7
        for i, j in day_map.items():
            if j == ans_day:
                ans_day = i
                break
        if total_day == 0:
            return time_stamp + ', ' + ans_day
        if total_day == 1:
            return time_stamp + ', ' + ans_day + ' (next day)'
        return time_stamp + ', ' + ans_day + ' (' + str(
            total_day) + ' days later)'
```


## [solution5](https://github.com/pkro/fcc_time_calculator/blob/main/time_calculator.py)

'''
add_time("3:00 PM", "3:10")
# Returns: 6:10 PM
add_time("11:30 AM", "2:32", "Monday")
# Returns: 2:02 PM, Monday
add_time("11:43 AM", "00:20")
# Returns: 12:03 PM
add_time("10:10 PM", "3:30")
# Returns: 1:40 AM (next day)
add_time("11:43 PM", "24:20", "tueSday")
# Returns: 12:03 AM, Thursday (2 days later)
add_time("6:30 PM", "205:12")
# Returns: 7:42 AM (9 days later)
'''
```python
full_day_minutes = 24 * 60
# no libs to be used, otherwise datetime would have these
weekdays = ['monday', 'tuesday', 'wednesday',
            'thursday', 'friday', 'saturday', 'sunday']


def split_time(time_str, use24=True):
    """ returns a tuple of hours and minutes of a given string in the form of "8:24" or "8:24 AM"; 
        adds +12 to hours if it detects " PM"
    Args:
        time_str (String): daytime string with optional am/pm indicator
        use24 (bool, optional): use 24h format for returned string. Defaults to True.
    """
    (h, m) = map(int, time_str.split(" ")[0].split(":"))
    try:
        period = time_str.split(" ")[1]
    except IndexError:
        period = ""
    if not use24 and h > 12:
        h = h - 12
        period = "PM"
    if time_str[-2:] == "PM":
        h = h + 12

    return (h, m, period)


def make_time(minutes):
    """returns time, period, days added from minutes
    Args:
        minutes (int): minutes
    """
    days_added = 0
    while minutes > full_day_minutes:
        minutes -= full_day_minutes
        days_added += 1

    h = minutes // 60
    m = minutes % 60

    if h >= 0 and (h < 12 or h == 12 and m == 0):
        period = "AM"
    else:
        period = "PM"

    if h > 13:
        h = h - 12

    if h == 0 and period == "AM":
        h = 12
    return (h, m, period, days_added)


def pad_time(h, m):
    """pads minutes in h:m string
    Args:
        h (int): hours
        m (int): minutes
    Returns:
        striung: time string with minutes padded
    """
    return f'{str(h)}:{str(m).zfill(2)}'


def weekday_after(start_day, days=0):
    if start_day == "":
        return ""
    weekday = start_day.lower()
    weekday_idx = weekdays.index(weekday)
    weekday_idx = weekday_idx + days
    new_weekday_idx = weekday_idx % 7
    return weekdays[new_weekday_idx].capitalize()


def time_in_minutes(iterable):
    return iterable[0] * 60 + iterable[1]


def get_days_later(days):
    days_later = ""
    if days > 1:
        days_later = f' ({days} days later)'
    if days == 1:
        days_later = ' (next day)'
    return days_later


def add_time(start, duration, weekday=""):

    (start_h, start_m, period) = split_time(start)
    start_minutes = time_in_minutes([start_h, start_m])
    duration_split = split_time(duration)
    duration_minutes = time_in_minutes(duration_split)
    (end_h, end_m, period, days_added) = make_time(
        start_minutes + duration_minutes)

    new_weekday = weekday_after(weekday, days_added)
    if new_weekday:
        new_weekday = f', {new_weekday}'

    days_later = get_days_later(days_added)

    new_time = f'{pad_time(end_h, end_m)} {period}{new_weekday}{days_later}'
    return new_time
```