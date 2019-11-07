# censusdata
# School Census Data
## Group Six-th Sense

### The original decision back in 2015 involved "flagging" unusual responses by creating a separate variable. We chose to stick with the same method, but changed the decisions that were made in 2015 about the scoring of some of the variables.

### 0 = no problem/not altered 1 = slightly unusual 2 = extremely unusual (likely a problem)

## Ageyears 
## ages 5 and younger and ages 23 and older were flagged as extremely unusual. Ages between 6 and 8 and ages between 20 and 22 were also flagged as slightly unusual.
Census$Ageyears <- Census$Age_years
Census$flag_age_years <- 0
Census$flag_age_years[which(Census$Ageyears <= 5 | Census$Ageyears >= 23)] <- 2
Census$flag_age_years[which(Census$Ageyears >= 6  & Census$Ageyears <= 8)] <- 1
Census$flag_age_years[which(Census$Ageyears >= 20 & Census$Ageyears <= 22)] <- 1


## ClassSize 
### A class size of more than 60 was flagged. Some states have laws prohibiting large class sizes in schools. After plotting the data a decision was made to make 60 the cut-off value.
Census$flag_classsize <- 0
Census$flag_classsize[which(Census$ClassSize >= 60 | Census$ClassSize < 1)] <- 2


## Languages_spoken
### We categorized more than 15 languages as unlikely for school-aged children. Between 10 and 15 languages was also flagged.
Census$Languages_spoken[Census$Languages_spoken == 0] <-1
Census$flag_languages_spoken <- 0
Census$flag_languages_spoken[which(Census$Languages_spoken >15)] <- 2
Census$flag_languages_spoken[which(Census$Languages_spoken >=10 & Census$Languages_spoken <=15)] <- 1


##3Reaction_time 
### We kept reaction time the same.
Census$flag_reaction_time <- 0
Census$flag_reaction_time[which(Census$Reaction_time < 0.101)] <- 2
Census$flag_reaction_time[which(Census$Reaction_time >1.21)] <- 2


##3Travel_time_to_School
### We changed the travel times that would be flagged as 1, or "slightly unusual", to between 75 minutes and 120 minutes because in big cities with traffic and rural areas with lack of schools, travel times could feasibly be within this interval.
Travel_time_to_School<-Census$Travel_time_to_school
Travel_time_to_School[Travel_time_to_School==0.01]<-6
Travel_time_to_School[Travel_time_to_School==0.23]<-14
Travel_time_to_School[Travel_time_to_School==0.316]<-19
Travel_time_to_School[Travel_time_to_School==0.34]<-20
Travel_time_to_School[Travel_time_to_School==0.37]<-22
Travel_time_to_School[Travel_time_to_School==0.405]<-24
Travel_time_to_School[Travel_time_to_School==0.499]<-30
Travel_time_to_School[Travel_time_to_School==0.5]<-30
Travel_time_to_School[Travel_time_to_School==0.69]<-41
Travel_time_to_School[Travel_time_to_School==0.75]<-45
Travel_time_to_School[Travel_time_to_School==0.818]<-49
Travel_time_to_School[Travel_time_to_School==1]<-60
Census$flag_Travel_time_to_School <- 0
Census$flag_Travel_time_to_School[which(Travel_time_to_School>120)]<- 2
Census$flag_Travel_time_to_School[which(Travel_time_to_School>75 & Travel_time_to_School<=120)]<- 1
Census$flag_Travel_time_to_School[which(Travel_time_to_School<=60)]<-0


##Score_in_memory_game - We kept memory game scores the same
Census$flag_score_in_memory_game <- 0
Census$flag_score_in_memory_game[which(Census$Score_in_memory_game < 2.02 |Census$Score_in_memory_game > 96)] <- 2


## Height_cm_cm 
### We increased the cutoff of unusual height and armspan to greater than 1
Height_cm <- Census$Height_cm
Height_cm[Height_cm==""]<-NA
Height_cm[Height_cm=="+9000"]<-NA
Height_cm[Height_cm==".5"]<-NA
Height_cm[Height_cm==".30"]<-NA
Height_cm_cm<-as.numeric(Height_cm)
Height_cm[Height_cm<=1]<-NA
Height_cm[which(Height_cm<=2.35)]<-100*(Height_cm[which(Height_cm<2.35)])   #m-cm
Height_cm[which(Height_cm>2.35&Height_cm<3.1)]<-NA
Height_cm[which(Height_cm>=3.1&Height_cm<=7.7)]<-30.48*(Height_cm[which(Height_cm>=3.1&Height_cm<=7.7)])  #foot-cm
Height_cm[which(Height_cm>7.7&Height_cm<37.4)]<-NA
Height_cm[which(Height_cm>=37.4&Height_cm<=92.5)]<-2.54*(Height_cm[which(Height_cm>=37.4&Height_cm<=92.5)])  #inch-cm
Height_cm[which(Height_cm>92.5&Height_cm<95)]<-NA
Height_cm[which(Height_cm>235&Height_cm<950)]<-NA
Height_cm[which(Height_cm>=950&Height_cm<=2350)]<-0.1*(Height_cm[which(Height_cm<=2350&Height_cm>=950)])   #mm-cm
Height_cm[which(Height_cm>2350)]<-NA

Armspan_cm <- Census$Armspan_cm
Armspan_cm[Armspan_cm<1]<-NA
Armspan_cm[which(Armspan_cm<=2.35)]<-100*(Armspan_cm[which(Armspan_cm<2.35)])   #m-cm
Armspan_cm[which(Armspan_cm>2.35&Armspan_cm<3.1)]<-NA
Armspan_cm[which(Armspan_cm>=3.1&Armspan_cm<=7.7)]<-30.48*(Armspan_cm[which(Armspan_cm>=3.1&Armspan_cm<=7.7)])  #foot-cm
Armspan_cm[which(Armspan_cm>7.7&Armspan_cm<37.4)]<-NA
Armspan_cm[which(Armspan_cm>=37.4&Armspan_cm<=92.5)]<-2.54*(Armspan_cm[which(Armspan_cm>=37.4&Armspan_cm<=92.5)])  #inch-cm
Armspan_cm[which(Armspan_cm>92.5&Armspan_cm<95)]<-NA
Armspan_cm[which(Armspan_cm>235&Armspan_cm<950)]<-NA
Armspan_cm[which(Armspan_cm>=950&Armspan_cm<=2350)]<-0.1*(Armspan_cm[which(Armspan_cm<=2350&Armspan_cm>=950)])   #mm-cm
Armspan_cm[which(Armspan_cm>2350)]<-NA

Armspan_cm[which(is.na(Armspan_cm))]<-Height_cm[which(is.na(Armspan_cm))]
Height_cm[which(is.na(Height_cm))]<-Armspan_cm[which(is.na(Height_cm))]

Census$Height_cm<-Height_cm
Census$Armspan_cm<-Armspan_cm

## Footlength_cm
### Based on research of footsizes for children between the ages of 8 and 20, we chose 20 centimeters as the minimum cut-off value and 34 centimeter as the maximum cut-off value. We kept the same conversion calculations as the previous group and replaced the cut-off values.
Footlength_cm<-Census$Footlength_cm
Footlength_cm[which(Footlength_cm<0.2)]<-NA 
Footlength_cm[which(Footlength_cm<=0.34&Footlength_cm>=0.2)]<-100*(Footlength_cm[which(Footlength_cm<=0.34&Footlength_cm>=0.2)])   #m-cm
Footlength_cm[which(Footlength_cm>0.65&Footlength_cm<=1.1)]<-30.48*(Footlength_cm[which(Footlength_cm>0.65&Footlength_cm<=1.1)])  #foot-cm
Footlength_cm[which(Footlength_cm>1.31&Footlength_cm<3.5)]<-NA
Footlength_cm[which(Footlength_cm>=7.9&Footlength_cm<=13.4)]<-2.54*(Footlength_cm[which(Footlength_cm>=7.9&Footlength_cm<=13.4)])  #inch-cm
Footlength_cm[which(Footlength_cm>34&Footlength_cm<90)]<-NA
Footlength_cm[which(Footlength_cm>=90&Footlength_cm<=340)]<-0.1*(Footlength_cm[which(Footlength_cm<=400&Footlength_cm>=90)])   #mm-cm
Footlength_cm[which(Footlength_cm>340)]<-NA


#Importance_reducing_pollution,Importance_recycling_rubbish,Importance_conserving_water,Importance_saving_energy,Importance_owning_computer,Importance_Internet_access 
## We left these variables the same
importance_function = function(variable, option = 1){
  flag = rep(0, length(variable))
  var_new = variable
  combo = cbind.data.frame(variable, flag, var_new)
  for(i in (1:length(variable))){
    if(is.na(variable[i]) == F){
      if(variable[i] > 1000 | variable[i] < 0){
        combo$flag[i] = 2
        combo$var_new[i] = NA
      }
    }
  }
  if(option == 1){
    return(combo$flag)
  }
  else if(option == 2){
    return(combo$var_new)
  }
}
Census$flag_Importance_reducing_pollution = importance_function(Census$Importance_reducing_pollution, option = 1)
Census$flag_Importance_recycling_rubbish = importance_function(Census$Importance_recycling_rubbish, option = 1)
Census$flag_Importance_conserving_water = importance_function(Census$Importance_conserving_water, option = 1)
Census$flag_Importance_saving_energy = importance_function(Census$Importance_saving_energy, option = 1)
Census$flag_Importance_owning_computer = importance_function(Census$Importance_owning_computer, option = 1)
Census$flag_Importance_Internet_access = importance_function(Census$Importance_internet_access, option = 1)
