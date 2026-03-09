# loglens  
Natural Language Debugging Assistant Built On AlloyDB And Gemini  
  
Data Flow for LogLens :  
&nbsp;&nbsp;&nbsp;&nbsp;The natural langauge question arrives at Flask API (inside api/)     
&nbsp;&nbsp;&nbsp;&nbsp;The API passes the question to the engine (inside engine/)  
&nbsp;&nbsp;&nbsp;&nbsp;The engine checks : is this a simple query or complex one?   
&nbsp;&nbsp;&nbsp;&nbsp;If simple -> sends question to AlloyDB AI, which generates and runs SQL directly   
&nbsp;&nbsp;&nbsp;&nbsp;If complex -> sends question + database schema to Gemini, which generates SQL, then the engine runs that SQL against the database  
&nbsp;&nbsp;&nbsp;&nbsp;The result comes back to the engine  
&nbsp;&nbsp;&nbsp;&nbsp;The engine sends the results to Gemini again, which generates 3 follow-up questions  
&nbsp;&nbsp;&nbsp;&nbsp;Everything - SQL, results, follow-ups goes back to the Flask API  
&nbsp;&nbsp;&nbsp;&nbsp;The Flask API sends it to the React Frontend  
&nbsp;&nbsp;&nbsp;&nbsp;The frontend displays it to the engineer  
