# git-server-docker
A lightweight Git Server Docker image built with Alpine Linux. Available on [GitHub](https://github.com/jkarlosb/git-server-docker) and [Docker Hub](https://hub.docker.com/r/jkarlos/git-server-docker/)

!["image git server docker" "git server docker"](https://raw.githubusercontent.com/jkarlosb/git-server-docker/master/git-server-docker.jpg)

## Basic Usage

This repository reles on `docker-compose`. First build your image(s):

	docker build -t crbroughton/git-server .

Note that the `docker-compose.yml` file includes `emarcs/nginx-cgit`, a
git front-end that run on port 8000 by default. If you don't require a
git front-end, simply remove this portion from the `docker-compose.yml` file.

Then run the container with `docker-compose`:

	docker-compose up -d

You can now find your git-server folder at `~/git-server`.
Place your public key in `~/git-server/keys` and then restart the container with:

	docker-compose restart

To check that your git server is running, run the below command:

	ssh git@<ip-docker-server> -p 2222

Which should output:

	Welcome to your dockerised git server!
	You've successfully authenticated!

	...

## Creating Bare Repositories

To create a bare repositiores, do the following:

	docker exec -it git-server /bin/sh
	cd repos && mkdir <REPO_NAME>.git && cd <REPO_NAME>.git && git init --bare

Once created, you'll now be able to clone the repository
from your server:

	git clone ssh://git@<ip-docker-server>:2222/git-server/repos/myrepo.git

## Raspberry Pi - Known issues

When pushing a repository to a Raspberry Pi, the below error can occur:

	error: remote unpack failed: unable to create temporary object directory

To fix, simply run the below command inside the `repos` folder on the server:

	sudo chown -R pi .

This will fix permissions issues.

