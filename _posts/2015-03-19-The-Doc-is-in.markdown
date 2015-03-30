---
layout: post
title:  "The Doc is in!"
date:   2015-03-19 08:00:25
categories: homework
tags: homework
image: /assets/images/desktopx2.jpg
---

**8.1.1**

I chose to comment through a recent project I did where I needed to quiz myself on a set of questions. The following are python modules I wrote to quiz myself. I only comment sections of code that aren't particularly clear and the heading comments for each method.

The main driver, driver.py:

{% highlight python %}

from filereader import read
from random import shuffle
import getch
import os
from subprocess import call
import platform

def main():
	"""
		This program is a quick tool to quiz yourself on 
		true/false, multiple choice, and open ended text based questions
	"""
	
	osClear()
	# Returns a list of filenames to be parsed
	selections = selectQuizzes()

	osClear()
	print("Learning mode automatically adds the questions you miss")
	print("back in to the quiz and shuffles the order of the questions")
	print("so that you learn all of the questions.")
	print("Do you want to turn on Learning Mode? (y/n)")

	# getch is a module that takes in a 1 char input without hitting the return key
	learningMode = getch.getch()
	if (learningMode.lower() == 'y'):
		learningMode = True

	# combine all the questions from the different txt files into one pool of questions
	for i in range(len(selections)):
		questions += read(os.path.join('Quizzes', selections[i]))

	# this does the actual quizzing and grading of responses
	if(len(questions) > 0):
		osClear()
		print(str(len(questions)) + " questions.")
		shuffle(questions)
		correct = 0
		total = 0
		scoreboard(correct, total, questions)
		print("")
		while(len(questions) > 0):
			total += 1
			question = questions.pop()
			question.toString()
			if(question.getType() != "fill_in_the_blank"):
				char = getch.getch()
				if (len(question.getAnswers()) <= ord(char)-97):
					print("Sorry, that is not a choice. Please try again.")
				while(len(question.getAnswers()) <= ord(char)-97):
					char = getch.getch()
				if(question.getAnswers()[ord(char)-97].lower() == question.getCorrect().lower()):
					osClear()
					correct += 1
					scoreboard(correct, total, questions)
					print("Correct" + "\n")
					print("")
				else:
					osClear()
					if(learningMode == True):
						questions.append(question)
						shuffle(questions)
					scoreboard(correct, total, questions)
					print("Incorrect")
					print("Answer: " + question.getCorrect() + "\n")
					print("")
			else:
				answer = raw_input()
				if(question.getCorrect().lower() == answer.lower()):
					osClear()
					correct += 1
					scoreboard(correct, total, questions)
					print("Correct \n")
					print("")
				else:
					osClear()
					if(learningMode == True):
						questions.append(question)
						shuffle(questions)
					scoreboard(correct, total, questions)
					print("Incorrect")
					print("Answer: " + question.getCorrect() + "\n")
					print("")
					
		score = round(float(correct) / total, 4)
		print ("Score: " + str(score * 100)) + "%"
	else:
		print("No Quizzes Found")


def osClear():
	"""
        performs the "clear" or "cls" command for whatever operating system
        its currently being run on.
    """
	if(platform.system() == 'Windows'):
		os.system('cls')
	else:
		call(["clear"])

def selectQuizzes():
	"""
		prompts the user to enter in the text files that they want to be quizzed
		on and returns their file names as a list
	"""
	quizzes = []
	selections = []
	for file in os.listdir("Quizzes"):
		if(file.endswith(".txt")):
			quizzes.append(file)
	if(len(quizzes) > 0):
		print("Type the numbers, separated by commas for the quizzes you wish to take or type all for all quizzes.")
		for i in range(1,len(quizzes)+1):
			print (str(i) + " " + quizzes[i-1])
		print("")
		selections = raw_input().split(',')
		if(selections == ['all']):
			selections.remove('all')
			for each in quizzes:
				selections.append(each)
		else:
			for i in range(len(selections)):
				selections[i] = quizzes[int(selections[i])-1]

	return selections


def scoreboard(correct, total, questions):
	"""
		processes the formatting for the scoreboard in the driver.
	"""
	left = len(questions)
	if(total == 0):
		score = 0
	else:
		score = round(float(correct) / total, 4)
	print("Correct: " + str(correct) + " | Questions Answered: " + str(total) + " | Questions Left: " + str(left) + " | Score: " + str(score * 100) + "%")
	print("")


if __name__ == '__main__':
	main()

{% endhighlight %}

and the text parser, filereader.py:

{% highlight python %}
from Question import Question


def read(filepath):
	"""
		takes in a filepath and treturns a list of question objects
	"""
	myFile = open(filepath, 'r')
	contents = myFile.read()

	questions = parseQuestions(contents)
	return makeQuestions(questions)

def parseQuestions(contents):
	"""
		separates the string version of the file contents into the individual questions
		and formats them to be easily made into Question objects
	"""
	questions = []
	question = ""
	flag = False
	
	for i in range(len(contents)):
		if(contents[i] == '\t'):
			continue
		if(contents[i-1] != '$' and contents[i] == '{'):
			flag = True
		elif(flag == True and contents[i] != '}'):
			question += contents[i]
		elif(contents[i] == '}' and (contents[i-1].isalpha() or contents[i-1] == ']')):
			question += contents[i]
		else:
			flag = False
			if(question != ""):
				questions.append(question)
				question = ""

	return questions

def makeQuestions(questions):
	"""
		Takes in a list of strings. Each string contains the information for one question object
	"""
	questionList = []
	for question in questions:
		sections = question.split('\n')
		for each in sections:
			if(each == ""):
				sections.remove(each)
		myQuestion = Question()
		condition = "True" in sections or "False" in sections
		if(len(sections) > 2 and not condition):
			myQuestion.setType(0)
		elif(len(sections) == 3):
			myQuestion.setType(1)
		else:
			myQuestion.setType(2)

		myQuestion.setQuestion(sections[0])
		for i in range(1,len(sections[1:])+1):
			if('#' in sections[i]):
				sections[i] = sections[i][0:len(sections[i])-2]
				myQuestion.setCorrect(sections[i])

		myQuestion.setAnswers(sections[1:])
		questionList.append(myQuestion)
	return questionList
{% endhighlight %}

