# Hive

Entire sandbox for the "Hive" application. This is a testing repo to test an entire cluster for an application with all the services needed, on separate repos, added as submodules in git

## Pre-requisites

* [git](https://git-scm.com/downloads) >= 1.8.2
* [dockers](https://www.docker.com/community-edition)

## Installation

To install, you need to clone the repo with `recurse-submodules` to pull the submodules as well. Check each submodule

```bash
git clone --recurse-submodules -j8 git@bitbucket.org:flukyfactory/hive.git
cd hive
docker-compose up --build
```

You will end up with a directory structure like this:

```
┣ hive
┣━ docker-compose.yml
┣┳ api <= this is a pointer to hive-api
┃┗━ Dockerfile
┗┳ front <= this is a pointer to hive-front
⬚┗━ Dockerfile
```

We now have the repo with submodules and a docker-compose with all(services) your app needs to run.

`docker-compose` will call each submodules' Dockerfile and build it service with is needed. For this example, it builds a container with GoLang and a container with NodeJS...this, to demonstrate how we can work with different languages and complex setups, easily.

## Usage

`docker-compose up --build` builds the containers' images, if you don\'t have them. to avoid this -and everytime you work on the project- just run:

```bash
docker-compose up
```

We now have the site running with a front(`localhost:8888`) and an API(`localhost:9999`) container.

## How to achive this

To get submodules the way they are here, you need to follow these steps:

```bash
# Create the new folder
  mkdir -p ~/sites/hive
  cd ~/sites/hive


# Add the given repositories as a submodule at the given path
  git submodule add git@github.com:pipa/micro2.git /micros/micro2
  git submodule add git@github.com:pipa/micro1.git /micros/micro1
  # git submodule [--quiet] add [-b <branch>] [-f|--force] [--name <name>] [--reference <repository>] [--] <repository> [<path>]

# To update submodules to latest sub-repo's branch tip
  git submodule update --recursive --remote
```

## Extras

We use some internals configs to the repo so, for this to work, you need to add the local `.gitconfig` setting in your git local configs.

```bash
git config --local include.path ../.gitconfig
```

Some of the helpers included in the local gitconfig:

```bash
git pu # pulls each submodule's branch and the main repo's branch
```

------------

The correct mental model to have is that the parent project "hive" points to a very specific revision of the submodules. Thus, if you make changes to the submodules' repo, these changes will not be visible in "hive" unless you update the pointer accordingly.

The recommended workflow is to never edit the contents of hive/submodule/ directly, but rather to update the submodule repo, and then to update the pointers in your hive, submodule, etc. repos.

Another key step here is that you must go into the submodule's directory and perform the git pull there, rather than in the root of the parent repo. Then you must return to the parent repo (i.e. be outside of the submodule) when you perform the commit and push.

------------

To remove submodules, here is the command

```bash
git submodule rm [submodule's name]
```

## Contributing

1. Fork it!
2. Create your feature branch: `git checkout -b my-new-feature`
3. Commit your changes: `git commit -am 'Add some feature'`
4. Push to the branch: `git push origin my-new-feature`
5. Submit a pull request :D

## Credits

TODO: Write credits

# Client directory structure goal:
```Markdown
Client - [master|staging|...n]
┣━ docker-compose.yml

┣━┳ MayanPrincess
┃⬚┣━ docker-compose.yml
┃⬚┣━┳ API
┃⬚┃⬚┗━ Dockerfile
┃⬚┣━┳ Front
┃⬚┃⬚┗━ Dockerfile
┃⬚┗━┳ Admin
┃⬚⬚⬚┗━ Dockerfile

┣━┳ REx
┃⬚┣━ docker-compose.yml
┃⬚┣━┳ API
┃⬚┃⬚┗━ Dockerfile
┃⬚┣━┳ Pagadito
┃⬚┃⬚┗━ Dockerfile
┃⬚┣━┳ JustGeo
┃⬚┃⬚┗━ Dockerfile
┃⬚┣━┳ Front
┃⬚┃⬚┗━ Dockerfile
┃⬚┗━┳ Admin
┃⬚⬚⬚┗━ Dockerfile

┣━┳ Galaxy
⬚⬚┣━ docker-compose.yml
⬚⬚┣━┳ API
⬚⬚┃⬚┗━ Dockerfile
⬚⬚┣━┳ Pagadito
⬚⬚┃⬚┗━ Dockerfile
⬚⬚┣━┳ JustGeo
⬚⬚┃⬚┗━ Dockerfile
⬚⬚┣━┳ Front
⬚⬚┃⬚┗━ Dockerfile
⬚⬚┗━┳ Admin
⬚⬚⬚⬚┗━ Dockerfile
```
