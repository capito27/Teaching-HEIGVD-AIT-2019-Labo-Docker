# Task 2

## Déliverables 
###2)

La réponse est fournie au niveau de la task 0, avec la M1 principalement.

La voici à nouveau : 


We can't use the current solution in a production environment.
This is due to the lack of a scaling mechanism. 
Indeed, the current 2 web app containers would instantly be at full load if they were to be deployed to production.
There are also no recovery systems in place if the application crashes, if the docker container goes down, the server won't be restarted, and the reverse proxy will lose the node until someone goes back and restart/recreate the container.

###3
