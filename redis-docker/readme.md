- separate conf for both master and slave
- if pod starts as a master it uses master configuration
- and if pod starts as slave, then slave configuration 
- Both configuration files have placeholders which are filled up while the pod is starting up
- Copying these two files in there respective directory
