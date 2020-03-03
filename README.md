# Python-Parsing-Project
# aFile=open("WorldPopulationData2019.txt", 'r')

# FUNCTION DEFINTION FOR EXECUTING PROGRAMME


#defining button click for each function
def button_click(x,y):
    if x<-150 and x>-300 and y>0 and y<150:
        total_population()
    elif x<10 and x>-140 and y>0 and y<150:
        percentage_changes()
    elif x<170 and x>20 and y>0 and y<150:
        top_change()
    elif x<330 and x>180 and y>0 and y<150:
        max_min_population()
        
#defining back button    
def back(x,y):
    if x>-300 and x<-250 and y>285 and y<300:
        main_menu(x,y)
        
#defining main menu
def main_menu(x,y):

    import turtle
    t=turtle.Turtle()


    menu() 
    turtle.onscreenclick(button_click,1)
    turtle.listen()
    turtle.mainloop()

#------------------------------------------------------------------------------------------------------------------------------------------------------------------------

#FIRST FUNCTION: TOTAL POPULATION BY CONTINENT
def total_population():
    import turtle
    aFile=open("WorldPopulationData2019.txt", encoding='ISO-8859-1')
    population={}
    for line in aFile:
        line=line.rstrip()
        data=line.split(";")
        try:
            num=int(data[0])
            if len(data)==8:
                continent=data[7]
                if continent in population:
                    population[continent]=population[continent]+int(data[3])
                else:
                    population[continent]=int(data[3])
        except:
            continue 
    for continent in population:
        print("Total population of", continent, "=",population[continent])
   
# BUILDING PIE CHART

#calculating the population of a continent as a proportion to total population
    sum_pop=0

    #iterates through each continent's population and calculates the total population of all continents (sum_pop)
    for continent in population:
        sum_pop=sum_pop + population[continent]
        
   

    proportion={}
    for continent in population:
        proportion[continent]=population[continent]/(sum_pop)


    #making the pie chart
    def piechart():
        
        t=turtle.Turtle()
        turtle.clearscreen()
        turtle.title("2019 Population")
        radius = 200
        angle=0
        colours = ["red","orange","yellow","pink","blue","green"]
        n=0

        #creating the sectors of the pie chart
        
        for continent in proportion:
            pie_chart_proportion=proportion[continent]*360
            t.setheading(angle)
            t.pencolor(colours[n])
            t.fillcolor(colours[n])
            t.begin_fill()
            t.pendown()
            t.forward(radius)
            t.left(90)
            t.circle(radius,pie_chart_proportion)
            t.home()
            t.end_fill()
            n=n+1
            angle=angle+pie_chart_proportion

#making the labels of the pie chart 
        for continent in proportion:
            pie_chart_proportion=proportion[continent]*360
            angle=angle+pie_chart_proportion/2 #to ensure that the labels are in the middle of each sector 
            t.penup()
            t.setheading(angle)
            t.forward(1.3*radius)
            t.write(continent.upper())
            t.forward(0.2*radius)
            value = "{:3.2f}".format(proportion[continent]*100)
            t.write(value + "%")
            angle=angle+pie_chart_proportion/2
            t.home()
        t.hideturtle()
        aFile.close()
        
    piechart()

    #designing back button 
    my_turtle = turtle.Turtle()
    my_turtle.hideturtle()
    my_turtle.penup()
    my_turtle.goto(-300,300)
    my_turtle.pendown()
    my_turtle.fillcolor("grey")
    my_turtle.begin_fill()
    for i in range(2):
        my_turtle.fd(50)
        my_turtle.rt(90)
        my_turtle.fd(15)
        my_turtle.rt(90)
    my_turtle.end_fill()
    my_turtle.penup()
    my_turtle.rt(90)
    my_turtle.fd(15)
    my_turtle.lt(90)
    my_turtle.fd(25)

    my_turtle.write("Back",align="center", font=("Ariel", 9, "bold"))

    turtle.onscreenclick(back,btn=1)

#---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

#SECOND FUNCTION


def percentage_changes():
    import turtle
    aFile=open("WorldPopulationData2019.txt", encoding='ISO-8859-1')
    percentage_change=[]
    print("{:35s}:{:20s},{:19s}".format("Country","Population in 2019","Percentage Change"))
    for line in aFile:
        line=line.rstrip()
        data=line.split(";")
        try:
            perc_change=float(data[6])
            pop_2019=int(data[3])
            country=data[1]
            percentage_change.append((country,pop_2019,perc_change))
            
        except:
            continue
    #function returns the second index of the list "percentage_change", which represents the percentage change in population
    def change(num):
        return num[2]
    percentage_change.sort(key=change, reverse = True)
    #prints the 
    for country,pop_2019,perc_change in percentage_change:
        print("{:35s}:{:20d},{:17.3f}".format(country,pop_2019,perc_change))



    #bar graph
    negative_percentages=[]
    # creating list of countries with negative percentage changes (from least negative to most negative) 
    for country,pop_2019,percentage in percentage_change:
        if float(percentage)<0:
            negative_percentages.append((country,percentage)) #creates a list of countries with negative percentage change in population 
        else:
            continue

    print("This function allows you to obtain a bar chart of countries with a negative population change") 
    n=input("Enter the number of countries you would like data for (max: 8):")
    x=1
    while True:
        if n.isdigit():
            if int(n)<=8:
                #yaxis
                turtle.clearscreen()
                y=turtle.Turtle()
                y.penup()
                y.goto(-300,-300)
                y.pendown()
                y.left(90)
                y.forward(500)
                y.write("PERCENTAGE CHANGE",font=("Times New Roman",12,"normal"))

                #xaxis
                x=turtle.Turtle()
                x.penup()
                x.goto(-300,0)
                x.pendown()
                x.goto(250,0)
                x.write("COUNTRY",font=("Times New Roman",12,"normal"))

                #drawing the graph 
                p=turtle.Turtle()
                p.penup()
                for i in range(-300,200,50): #labelling y axis
                    p.speed(9)
                    p.goto(-300,i)
                    p.forward(-20)
                    p.write(i/1000,font=("Times New Roman",10,"normal"))
                    p.forward(20)
                    p.pendown()
                    p.forward(550)
                    p.penup()
                    
                p.goto(-300,0) #turtle goes to origin
                
                for i in range(0,int(n)): #turtle plots graph for top n countries 
                      
                    distance=0.05*1000
                    p.penup()
                    p.forward(distance)
                    p.write(negative_percentages[i][0])
                    p.fillcolor("blue")
                    p.begin_fill()
                    p.pendown()
                    p.left(90)
                    p.forward(float((negative_percentages[i][1]))*1000)
                    p.penup()
                    p.forward(-0.03*1000)
                    p.write(str(negative_percentages[i][1])+"%",font=("Times New Roman",10,"normal"))
                    p.forward(0.03*1000)
                    p.pendown()
                    p.right(90)
                    p.forward(0.03*1000)
                    p.left(90)
                    p.forward(-float(negative_percentages[i][1])*1000)
                    p.left(90)
                    p.forward(0.03*1000)
                    p.right(180)
                    p.end_fill()
                    distance=distance+0.05

                p.hideturtle()
                break
            else:
                print("The number that has been inputted is too big. Try again!") #In case number is out of range 
                n=input("Enter the number of countries you would like data for (Max: 8):")
                continue
                
        else:
            print("Invalid input") #In case input is not a number 
            n=input("Enter the number of countries you would like data for (Max: 8):")
            continue

    # designing back button
    my_turtle = turtle.Turtle()
    my_turtle.hideturtle()
    my_turtle.penup()
    my_turtle.goto(-300,300)
    my_turtle.pendown()
    my_turtle.fillcolor("grey")
    my_turtle.begin_fill()
    for i in range(2):
        my_turtle.fd(50)
        my_turtle.rt(90)
        my_turtle.fd(15)
        my_turtle.rt(90)
    my_turtle.end_fill()
    my_turtle.penup()
    my_turtle.rt(90)
    my_turtle.fd(15)
    my_turtle.lt(90)
    my_turtle.fd(25)

    my_turtle.write("Back",align="center", font=("Ariel", 9, "bold"))

    turtle.onscreenclick(back,btn=1)



#---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


#THIRD FUNCTION

#What are the top n countries with the largest increase in population in 2019


def top_change():
    aFile=open("WorldPopulationData2019.txt", encoding='ISO-8859-1')
    percentage_change=[]
    for line in aFile:
        line=line.rstrip()
        data=line.split(";")
        try:
            perc_change=float(data[6])
            pop_2019=int(data[3])
            country=data[1]
            percentage_change.append((country,perc_change)) #creates list of countries with their percentage change in population 
        except:
            continue

    
    def change(num):
        return num[1] #returns the first index/population change 
    
    percentage_change.sort(key=change, reverse = True) #sorts using the first index/population change in the countries 
    print("This function will allow you to view the countries with the largest population change in 2019")
    n=input("Enter the number of coutnries you wish to collect data for:")
    count=0
    while True:
        if n.isdigit():#check if a number has been entered
            print("THE TOP",n,"COUNTRIES WITH LARGEST POPULATION CHANGE ARE:")
            for i in range(0,int(n)):
                    count=count+1
                    country=percentage_change[i][0]
                    percentage=percentage_change[i][1]
                    print(count, country,":",percentage,"%")
            break 
        else:
            print("Invalid input. Please try again") #in case input is not a number 
            n=input("Enter the number of coutnries you wish to collect data for:")
            continue #continues the loop after prompting user for another input 

    




#---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
#FOURTH FUNCTION

aFile=open("WorldPopulationData2019.txt", encoding="ISO-8859-1")

#For each continent, maximum and minimum population values must be presented  
def max_min_population():
    aFile=open("WorldPopulationData2019.txt", encoding='ISO-8859-1')
    #create an empty dictionary for maximum population
    max_population={}
    #create an empty dictionary for minimum population
    min_population={}
    #create an empty dictionary for country containing maximum population
    country_max={}
    #create an empty dictionary for country containing minimum population
    country_min={}

    for line in aFile:
        line=line.rstrip()
        #create a list of words in the line 
        data=line.split(";") 
        try:
            #checks whether the line contains a country with a population ranking 
            num=int(data[0])
            continent=data[7]
            
            #MAXIMUM VALUE 
            #checks if the continent data already exists in the dictionary
            if continent in max_population:
                if float(data[3])>float(max_population[continent]):
                    #updates new population data in the dictionary if it is greater than existing maximum population
                    max_population[continent]=float(data[3])
                    country_max[continent]=data[1]
            
            else:
                #if continent does not exist in dictionary, the continent and population data are added to the dictionary  
                max_population[continent]=float(data[3])
                country_max[continent]=data[1]


            #MINIMUM VALUE
            #checks if the continent data already exists in the dictionary
            if continent in min_population:
                if float(data[3])<float(min_population[continent]):
                    #updates new population data in the dictionary if it is greater than existing maximum population
                    min_population[continent]=float(data[3])
                    country_min[continent]=data[1]
            else:
                #if continent does not exist in dictionary, the continent and population data are added to the dictionary
                min_population[continent]=float(data[3])
                country_min[continent]=data[1]
        except:
            #eliminates the first two lines of the text file 
            continue 
    
    print("MAXIMUM AND MINIMUM POPULATION VALUES FOR EACH CONTINENT ARE:")
    for continent in max_population:
        print(continent.upper(),":")
        print("Minimum Population", "=",min_population[continent],"(",country_min[continent],")")
        print("Maximum Population","=",max_population[continent],"(",country_max[continent],")")




#MENU ------------------------------------------------------


def menu():
    
    import turtle
    colors=["sandy brown","gold","orange","salmon"]
    n=0
    
    turtle.clearscreen()    
    for i in range(-300,300,160):
        box=turtle.Turtle()
        box.penup()
        box.speed(9)
        box.goto(i,0)
        box.fillcolor(colors[n])
        box.begin_fill()
        box.pendown()
        box.forward(150)
        box.left(90)
        box.forward(150)
        box.left(90)
        box.forward(150)
        box.left(90)
        box.forward(150)
        box.end_fill()
        box.hideturtle()
        box.penup()
        n=n+1

    #Title

    box.goto(-200,250)
    box.write("CLICK ON THE BOX FOR THE INFORMATION YOU REQUIRE",20,font="Times")
    box.penup()


    #Designing first box to execute first function
    piecolor=['black','red','blue','green']
    angles=[45,50,135,130]
    box.goto(-225,75)
    n=0
    angle=0
    for index in angles:
                box.setheading(angle)
                box.pencolor(piecolor[n])
                box.fillcolor(piecolor[n])
                box.begin_fill()
                box.pendown()
                box.forward(50)
                box.left(90)
                box.circle(50,index)
                box.goto(-225,75)
                box.end_fill()
                n=n+1
                angle=angle+index
                box.penup()
                

    box.goto(-300,-30)
    box.write("2019 Population".upper(),font="Times")
    box.goto(-300,-50)
    box.write("by continent".upper(),font="Times")

    #Designing second box to execute second function
    box.goto(-85,25)
    box.pencolor("black")
    box.fillcolor("blue")
    box.begin_fill()
    box.pendown()
    box.forward(20)
    box.right(90)
    box.forward(20)
    box.right(90)
    box.forward(20)
    box.right(90)
    box.forward(20)
    box.end_fill()
    box.forward(-20)
    box.begin_fill()
    box.left(90)
    box.forward(-50)
    box.left(90)
    box.forward(20)
    box.right(90)
    box.forward(50)
    box.right(90)
    box.forward(20)
    box.end_fill()
    box.forward(-20)
    box.begin_fill()
    box.left(90)
    box.forward(-70)
    box.left(90)
    box.forward(20)
    box.right(90)
    box.forward(70)
    box.left(90)
    box.forward(-20)
    box.end_fill()
    box.penup()

    box.goto(-140,-30)
    box.write("PERCENTAGE CHANGE", font="Times")
    box.goto(-140,-50)
    box.write("IN POPULATION",font="Times")

    # Designing the third box
    box.pencolor("black")
    box.goto(20,-30)
    box.write("INCREASE IN POPULATION", font="Times")
    box.goto(20,-50)
    box.write("IN 2019", font="Times")
    box.penup()
    box.goto(120,70)
    box.write("+",font="Times")
    p=turtle.Turtle()
    p.penup()
    p.goto(85,20)
    p.left(90)
    p.pendown()
    p.forward(100)



    # Designing the fourth box
    box.goto(195,120)
    box.write("MAX",font="Times")
    box.goto(300,10)
    box.write("MIN", font="Times")
    box.goto(195,-30)
    box.write("MAXIMUM AND ", font="Times")
    box.goto(195,-50)
    box.write("MINIMUM ", font="Times")
    box.goto(195,-70)
    box.write("POPULATION DATA", font="Times")
    box.goto(195,-90)
    box.write("BY CONTINENT", font="Times")


main_menu(0,0)
    
      

    
   
    
                

        



    


