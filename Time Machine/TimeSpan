/*
TimeSpan cpp, implementation for TimeSpan header file
Author: Daniel Yan

*/

#include "stdafx.h"
#include <iostream>
#include "TimeSpan.h"
using namespace std;

//3 Parameter constructor that checks if inputted time is valid or not
TimeSpan::TimeSpan(double hours, double mins, double secs)
{

	setTime(hours, mins, secs);

}
//Constructor that only uses mins and secs (2 inputs)
TimeSpan::TimeSpan(double mins, double secs)
{
	setTime(0, mins, secs);
	
}

//Constructor that only uses secs (1 input)
TimeSpan::TimeSpan(double secs)
{
	setTime(0, 0, secs);
}

//Default constructor sets time values to 0
TimeSpan::TimeSpan()
{
	setTime(0, 0, 0);
}

//Copy Constructor
TimeSpan::TimeSpan(const TimeSpan &other)
{
	hours = other.getHours();
	mins = other.getMinutes();
	secs = other.getSeconds();
}

//Checks if inputted times is a positive or not
//Primarily used in overloaded streams for case checking
bool TimeSpan::isPositive(double hours, double mins, double secs) 
{
	double totalTimeInSeconds = (hours * 3600) + (mins * 60) + secs;

	//Checks if total time is negative
	if (totalTimeInSeconds >= 0)
	{
		return true;
	}
	else
	{
		return false;
	}
}

//Sets the time as needed
bool TimeSpan::setTime(double hours, double minutes, double seconds)
{	
	//Error checking is done by >> and << overloaded streams, so only a simple rounding method is called
	roundTime(hours, minutes, seconds);
	return true;
	//As there are no truly 'invalid' inputs, there are no false cases
}

//Getters
int TimeSpan::getHours() const
{	//As hours and this hours are the same name, return this hours
	return this->hours;
	
}

int TimeSpan::getMinutes() const
{
	return mins;
}

int TimeSpan::getSeconds() const
{
	return secs;
}

//Rounds and formats each time value in a normalized manner
void TimeSpan::roundTime(double hours, double minutes, double seconds)
{	

	double totalNumOfSeconds = hours * 3600 + minutes * 60 + seconds;
	hours = totalNumOfSeconds / 3600; minutes = 0; seconds = 0;
	//trunc reference: http://www.cplusplus.com/reference/cmath/trunc/
	//trunc() is used here in order to get separate the decimal value and calculate it in terms of minutes and seconds
	//trunc(hours) = int value of hours, and hours - trunc(hours) = decimal value of hours
	//trunc rounds to bottom int regardless if positive or negative
	minutes = (hours - trunc(hours)) * 60;
	//Rounds the seconds to nearest int 
	seconds = round((minutes - trunc(minutes)) * 60);
	
	
	//Conditionals to recalculate negative minutes and seconds
	//As negative times are okay, 0 to -59 times are allowed
	//Negative seconds (under -60), minus 1 minute
	if (seconds <= -60)
	{
		minutes--;
		seconds += 60;
	}

	//negative minutes, minus 1 hour
	if (minutes  <= -60 )
	{
		hours--;
		minutes += 60;

	}

	//over 60 seconds, + 1 minutes
	if (round(seconds) >= 60)
	{
		minutes ++ ;
		seconds -= 60;
	}

	 //over 60 minutes, + 1 hours
	if (round(minutes) >= 60)
	{
		hours++;
		minutes -= 60;
	}
	//Casts all values into ints to properly format the numbers	
	//this pointer
	this->hours = (int) hours;
	mins = (int) minutes;
	secs = (int) seconds;
}

//Operator Overloading

//<< Overloading: https://msdn.microsoft.com/en-us/library/1z2f6c2k.aspx - Used for general structure and formatting
//Structures and tips from http://courses.cms.caltech.edu/cs11/material/cpp/donnie/cpp-ops.html

ostream& operator<<(ostream &os, TimeSpan &ts)
{
	os.clear();
	//Prints out Hours, Minutes, and Seconds just like cout 
	//Hours cannot be under 0
	if (ts.getHours() >= 0)
	{
		os << "Hours: " << ts.getHours() <<
			", Minutes: " << ts.getMinutes() <<
			", Seconds: " << ts.getSeconds() << endl;
	}

	else
	{
		//Negative flipping for time
		ts.setTime(ts.getHours(), ts.getMinutes(), ts.getSeconds());
		//Negative time warning
		os << "A negative input has been detected" <<
			//Spacing to align format with prior ones
			"\n\n      Hours: " << ts.getHours() <<
			", Minutes: " << ts.getMinutes() <<
			", Seconds: " << ts.getSeconds() << endl;

	}

	return os;
}
// Used https://stackoverflow.com/questions/20446373/cin-ignorenumeric-limitsstreamsizemax-n-max-not-recognize-it for the numeric_limits
// Avoids multiple warnings from flagging the fail bit multiple times
istream& operator>>(istream &is,TimeSpan &ts)
{	
	double hours, minutes, seconds;
	cout << "Input Hours, Minutes, and Seconds " << endl;
	is >> hours >> minutes >> seconds;
	if (ts.isPositive(hours, minutes, seconds) && !(is.fail()))
	{
		ts.setTime(hours, minutes, seconds);
	}
	else
	{
		while (!(ts.isPositive(hours, minutes, seconds)) || is.fail())
		{
			//Input has a negative time and has no invalid input
			if (!(ts.isPositive(hours, minutes, seconds)) && !cin.fail())
			{
				int choice = 0;
				//Prompt for choice
				cout << "Your input was a negative time, select the following options to correct it.\n"
					<< "Press (1) to flip the time into a positive time\n"
					<< "Press (2) to reinput the time" << endl;
				//Clears prior stream and ignores multiple inputs
				is.clear();
				cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
				is >> choice;
				//Choice is given
				switch (choice)
				{
				case 1:
					ts.setTime(-hours, -minutes, -seconds);
					if (!cin.fail())
					{
						//Returns a valid inputstream
						return is;
					}
					break;
				case 2:
					cout << "Please enter your input for Hours, Minutes, and Seconds again" << endl;
					is >> hours >> minutes >> seconds;
				}

			}

			//Input is invalid, but not negative
			else
			{
				is.clear();
				cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
				cout << "Please enter your input for Hours, Minutes, and Seconds again" << endl;
				is >> hours >> minutes >> seconds;

			}

		}
		//Sets the time to the proper amount if valid
		ts.setTime(hours, minutes, seconds);
	}
	//Returns inputstream
	return is;
}

//Binary Operators
TimeSpan TimeSpan::operator+(const TimeSpan &ts) const
{	
	//Copy Constructor returned with new add value of ts
	return TimeSpan(*this) += ts;
}

TimeSpan TimeSpan::operator-(const TimeSpan &ts) const
{
	return TimeSpan(*this) -= ts;
}

//Assignment Operator
TimeSpan & TimeSpan::operator=(const TimeSpan &ts)
{
	
	//Checks if object is the same to avoid self-assignment
	if (this != &ts)
	{
		//Assigns var to equal ts object value
		hours = ts.getHours();
		mins = ts.getMinutes();
		secs = ts.getSeconds();
	}
	

	return *this;
}

//Compound Operators
TimeSpan & TimeSpan::operator+=(const TimeSpan &ts)
{
	hours += ts.getHours();
	mins += ts.getMinutes();
	secs += ts.getSeconds();
	roundTime(hours, mins, secs);
	return *this;
}

TimeSpan & TimeSpan::operator-=(const TimeSpan &ts)
{
	hours -= ts.getHours();
	mins -= ts.getMinutes();
	secs -= ts.getSeconds();
	setTime(hours, mins, secs);
	return *this;
}

//Unary negator
TimeSpan TimeSpan::operator-() const
{	
	//Reverses value signs
	TimeSpan ts;
	ts.hours = -hours;
	ts.mins = -mins;
	ts.secs = -secs;
	return ts;
}

//Boolean opeartors

bool TimeSpan::operator==(const TimeSpan &ts)
{
	if (hours == ts.getHours()
		&& mins == ts.getMinutes()
		&& secs == ts.getSeconds())
		return true;
	else
		return false;

}

bool TimeSpan::operator!=(const TimeSpan &ts)
{	
	//Uses == operator overloaded
	return !(*this == ts);
}
