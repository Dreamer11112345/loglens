# loglens
Natural Language Debugging Assistant Built On AlloyDB And Gemini

Data Flow for LogLens :
The natural langauge question arrives at Flask API (inside api/)
The API passes the question to the engine (inside engine/)
The engine checks : is this a simple query or complex one?
If simple -> sends question to AlloyDB AI, which generates and runs SQL directly
If complex -> sends question + database schema to Gemini, which generates SQL, then the engine runs that SQL against the database
The result comes back to the engine
The engine sends the results to Gemini again, which generates 3 follow-up questions
Everything - SQL, results, follow-ups goes back to the Flask API
The Flask API sends it to the React Frontend
The frontend displays it to the engineer
