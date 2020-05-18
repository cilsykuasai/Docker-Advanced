# Assignment: Create A Multi-Service Multi-Node Web App

## Goal: create networks, volumes, and services for a web-based "cats vs. dogs" voting app.

- See architecture.png in this directory for a basic diagram of how the 5 services will work
- All images are on Docker Hub, so you should use editor to craft your commands locally, then paste them into swarm shell.
- a `backend` and `frontend` overlay network are needed. Nothing different about them other then that backend will help protect database from the voting web app. (similar to how a VLAN setup might be in traditional architecture)
- The database server should use a named volume for preserving data. Use the new `--mount` format to do this: `--mount type=volume,source=db-data,target=/var/lib/postgresql/data`

### Services (names below should be service names)
- vote
    - web front end for users to vote dog/cat
    - use image : dockersamples/examplevotingapp_vote:before
    - ideally published on TCP 80. Container listens on 80
    - on frontend network
    - 2+ replicas of this container

- redis
    - key/value storage for incoming votes
    - use image : redis:3.2
    - no public ports
    - on frontend network
    - 1 replica

- worker
    - backend processor of redis and storing results in postgres
    - use image : dockersamples/examplevotingapp_worker
    - no public ports
    - on frontend and backend networks
    - 1 replica

- db
    - for database storing
    - one named volume needed, pointing to /var/lib/postgresql/data
    - use image : postgres:9.4
    - on backend network
    - 1 replica

- result
    - web app that shows results
    - use image : dockersamples/examplevotingapp_result:before
    - runs on high port since just for admins (lets imagine)
    - so run on a high port like 5001, container listens on 80
    - on backend network
    - 1 replica
