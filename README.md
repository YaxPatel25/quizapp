This is a basic quiz app based on the spring boot. I used Postman & Pg admin 4(PostgreSQL). The project follows MVC pattern. 

Here is the structure of the project:
Default package:
  - It contains main function to start the spring application.

Controller:
  - This package handles the requests and connects service layer in order to perform the business logic.

Service:
  -  This layer implements the business logic.
  -  We need to use try/catch to handle bad requests.
  -  QuestionService:
      1. getAllQuestions: It will return all questions from the DB with the help findAll(Predefined method) method.
      2. getQuestionByCategory: Return the list of questions with the given category. Implement the findByCategory function with the help of JpaRepository.
      3. addQuestion: It saves the Question entity with the help of predefined save() function.
      4. deleteQuestion: Delete the questions with the given id.
  -  QuizService:
      1. createQuiz: It will generates quiz of random question with the help of findRandomQuestionByCategory() method which id defined in QuestionDao. The function is implemented nativeQuery which customize the result according to need. After that, It will set the title and questions in the quiz object. And, the quiz object will be saved by save() method. At the end, quiz and quiz_questions table will be created.
      2. getQuizQuestions: It will first fetch the quiz with the given ID. After that, list of questions for the quiz will be stored in questionsFromDB(@ManyToMany). Then, the for loop will assign all the needed column to questionsForUser from questionsFromDB one by one.
      3. calculateResult: We need to pass ID of the question and List of Reponse in the parameters. We need to provide Reponse in postman to certain format which is defined in the Model package. The particular quiz with the given ID will be fetched. And then, list of questions will be fetched for the particular quiz. Now, We wil compare value of the responses from the postman request with questions rightAnswers by using for loop.

Model: 
  - Question: @Data & @Entity used for getters & setters and to connect with table in postgreSQL.
  - QuestionWrapper: It will save the quiz question requested by users. It will store only required elements to perform the quiz.
  - Quiz: @Entity is used to connect with the Quiz table.  @ManyToMany is used to map the table and will generate another quiz_questions table.
  - Response: It will save the request from postman with id(Integer) and response(String). @RequiredArgsConstructor is used to eliminate boilerplate code for the constructor.

Dao:
  - This layer will helps to connect with database with the specified table in the JpaRepository with primary key.
  - QuestionDao: It connects Question table and take primary key type as argument.
      1. findByCategory: This function will get the category and return List of questions by the category. This method is also predefined in the JpaRepository.
      2. findRandomQuestionByCategory: This function uses naticeQuery to customize the output according to requirement.
  -  QuizDao: It connects Quiz table and take primary key type as argument.
