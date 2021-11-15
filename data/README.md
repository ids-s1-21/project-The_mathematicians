---
output:
  word_document: default
  html_document: default
---
# Data sets

The data analysed by the project is `responses_joined_completed.csv` which is drawn from the source data `ANON_MAPS_responses_1920_S1.csv`, `demographics.csv` and `maps_questions_processed.csv`.

## Format and variables

`ANON_MAPS_responses_1920_S1.csv` is a csv file with 677 rows and 35 columns.  
Each row/observational unit represents the answers to the survey from a unique student.  
Each column is a variable, they are:  
  - `AnonID`: The unique ID generated for each survey filled by student  
  - `Date`: The date when student completed the survey  
  - *Questions*: Columns 3-35 represent all questions (question 0-32). The name of the variable is the descriptive part of each question, they are listed below:  
     [1] "After.I.study.a.topic.in.math.and.feel.that.I.understand.it,.I.have.difficulty.solving.problems.on.the.same.topic."                                                            
     [2] "There.is.usually.only.one.correct.approach.to.solving.a.math.problem."                                                                                                         
     [3] "I'm.satisfied.if.I.can.do.the.exercises.for.a.math.topic,.even.if.I.don't.understand.how.everything.works."                                                                    
     [4] "I.do.not.expect.formulas.to.help.my.understanding.of.mathematical.ideas,.they.are.just.for.doing.calculations."                                                                
     [5] "Math.ability.is.something.about.a.person.that.cannot.be.changed.very.much."                                                                                                    
     [6] "Nearly.everyone.is.capable.of.understanding.math.if.they.work.at.it."                                                                                                          
     [7] "Understanding.math.means.being.able.to.recall.something.you've.read.or.been.shown."                                                                                            
     [8] "If.I.am.stuck.on.a.math.problem.for.more.than.ten.minutes,.I.give.up.or.get.help.from.someone.else."                                                                           
     [9] "I.expect.the.answers.to.math.problems.to.be.numbers."                                                                                                                          
    [10] "If.I.don't.remember.a.particular.formula.needed.to.solve.a.problem.on.a.math.exam,.there's.nothing.much.I.can.do.to.come.up.with.it."                                          
    [11] "In.math,.it.is.important.for.me.to.make.sense.out.of.formulas.and.procedures.before.I.use.them."                                                                               
    [12] "I.enjoy.solving.math.problems."                                                                                                                                                
    [13] "Learning.math.changes.my.ideas.about.how.the.world.works."                                                                                                                     
    [14] "I.often.have.difficulty.organizing.my.thoughts.during.a.math.test."                                                                                                            
    [15] "Reasoning.skills.used.to.understand.math.can.be.helpful.to.me.in.my.everyday.life."                                                                                            
    [16] "To.learn.math,.the.best.approach.for.me.is.to.memorize.solutions.to.sample.problems."                                                                                          
    [17] "No.matter.how.much.I.prepare,.I.am.still.not.confident.when.taking.math.tests."                                                                                                
    [18] "It.is.a.waste.of.time.to.understand.where.math.formulas.come.from."                                                                                                            
    [19] "We.use.this.statement.to.discard.the.survey.of.people.who.are.not.reading.the.questions..Please.select.Agree.(not.Strongly.Agree).for.this.question."                          
    [20] "I.can.usually.figure.out.a.way.to.solve.math.problems."                                                                                                                        
    [21] "School.mathematics.has.little.to.do.with.what.I.experience.in.the.real.world."                                                                                                 
    [22] "Being.good.at.math.requires.natural.(i.e..innate,.inborn).intelligence.in.math."                                                                                               
    [23] "When.I.am.solving.a.math.problem,.if.I.can.see.a.formula.that.applies.then.I.don't.worry.about.the.underlying.concepts."                                                       
    [24] "If.I.get.stuck.on.a.math.problem,.there.is.no.chance.that.I.will.figure.it.out.on.my.own."                                                                                     
    [25] "When.learning.something.new.in.math,.I.relate.it.to.what.I.already.know.rather.than.just.memorizing.it.the.way.it.is.presented."                                               
    [26] "I.avoid.solving.math.problems.when.possible."                                                                                                                                  
    [27] "I.think.it.is.unfair.to.expect.me.to.solve.a.math.problem.that.is.not.similar.to.any.example.given.in.class.or.the.textbook,.even.if.the.topic.has.been.covered.in.the.course."
    [28] "All.I.need.to.solve.a.math.problem.is.to.have.the.necessary.formulas."                                                                                                         
    [29] "I.get.upset.easily.when.I.am.stuck.on.a.math.problem."                                                                                                                         
    [30] "Showing.intermediate.steps.for.a.math.problem.is.not.important.as.long.as.I.can.find.the.correct.answer."                                                                      
    [31] "For.each.person,.there.are.math.concepts.that.they.would.never.be.able.to.understand,.even.if.they.tried."                                                                     
    [32] "I.only.learn.math.when.it.is.required."  
    
  (Quotes and index number are not contained in the dataset)  

`demographics.csv` is a csv file with 1361 rows and 3 columns. Each row/observational unit is a unique student.   
Each column is a variable, they are:  
  - `anon_id`: The unique ID generated for each student (not necessarily filled the survey)    
  - `gender`: The gender of the student ("F" for females and "M" for males")  
  - `programme_school_name`: The school where the student belongs to, which includes the following:  
     [1] "School of Mathematics"                                 
     [2] "School of Informatics"                                 
     [3] "School of Divinity"                                    
     [4] "School of Physics and Astronomy"                       
     [5] "Business School"                                       
     [6] "School of Economics"                                   
     [7] "School of Social and Political Science"                
     [8] "Deanery of Biomedical Sciences"                        
     [9] "School of Literatures, Languages and Cultures"         
    [10] "School of Philosophy, Psychology and Language Sciences"
    [11] "School of Geosciences"                                 
    [12] "School of Biological Sciences"                         
    [13] "School of Chemistry"                                   
    [14] "School of Engineering"                                 
    [15] "Edinburgh College of Art"                              
    [16] "Moray House School of Education and Sport"             
    [17] "School of History, Classics and Archaeology"           
    [18] "College of Arts, Humanities and Social Sciences"       
    [19] "School of Health in Social Science"        
    
     (Quotes and index number are not contained in the dataset)  

`maps_question_processed.csv` is a csv file with 32 rows/observational units and 5 columns/variables.  
Each row/observational unit represents the information related to a particular question and variables are listed as followed:  
    - `...1`: Index for each observational unit automatically generated by R  
    - `raw`: Shows how the question is presented in the original survey  
    - `qnum`: The number of question (32 questions in total and 1 extra question asking for consent)  
    - `expected_ans`: The expected answer to each question given by experts  
    - `qtext`: The descriptive of each question (A part of `raw`)  

`responses_joined_completed.csv` is a csv file with 22341 row/observational units and 10 columns/variables.  
    - `anon_id`: The unique ID generated for each survey filled by student  
    - `Date`: The date when student completed the survey  
    - `programme_school_name`: The school which the student belong to  
    - `gender`: The gender of the student  
    - `answer`: The answer to each question (Agree, Strongly Agree, Neutral, Disagree, Strongly Disagree)  
    - `qnum`: The number of question (32 questions in total and 1 extra question asking for consent)  
    - `...7`: An index automatically generated by R for each question  
    - `raw` : Shows how the question is presented in the original survey  
    - `expected_ans`: The expected answer to each question given by experts  
    - `qtext`: The descriptive of each question (A part of `raw`)  
      


