# loglens  
Natural Language Debugging Assistant Built On AlloyDB And Gemini  
  
#### Data Flow for LogLens :  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The natural langauge question arrives at Flask API (inside api/)     
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The API passes the question to the engine (inside engine/)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The engine checks : is this a simple query or complex one?   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;If simple -> sends question to AlloyDB AI, which generates and runs SQL directly   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;If complex -> sends question + database schema to Gemini, which generates SQL, then the engine runs that SQL against the database  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The result comes back to the engine  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The engine sends the results to Gemini again, which generates 3 follow-up questions  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Everything - SQL, results, follow-ups goes back to the Flask API  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The Flask API sends it to the React Frontend  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The frontend displays it to the engineer  

#### Query is Simple or Complex? How to decide?
The question is passed to the engine  
The engine needs to decide simple or complex from the question itself which is an English sentence  
The engine cannot work with SQL to classify query into simple or complex here because the SQL does not exist when the decision is to be made  
The routing decision can be made by made by engine using LLM - Gemini  
The Gemini takes the English question and the schema of all tables as input and then decides whether the query is simple or complex  
This is called as Intent Classification  
The engine then uses the answer to decide the routing path  

#### Full Routing Path :
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Simple Query Flow :  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Question + Schema -> Gemini -> Returns Classification (simple)    
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Question -> AlloyDB AI -> Generates SQL -> Returns Result  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Question + Result + Schema -> Gemini -> 3 Follow Up Questions  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Result + 3 Follow Up Questions -> Displayed to Engineer
  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Complex Query Flow :  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Question + Schema -> Gemini -> Returns Classification (complex) + SQL  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SQL -> Database -> Returns Results  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Question + Result + Schema -> Gemini -> 3 Follow Up Questions  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Result + 3 Follow Up Questions -> Displayed to Engineer  
This is the logic my engine/ holds  

####The Trade Offs :
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Simple Query Workflow requires 1 Gemini call for Classification, 1 AlloyDB AI call for Querying Database, 1 Gemini call for Follow Up Questions - 2 Gemini calls, 1 AlloyDB AI call
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Complex Query Workflow requires 1 Gemini call for Classification + SQL generation
