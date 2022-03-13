# [Git](https://git-scm.com/)

## Docker image

[docker-scikit-learn](https://hub.docker.com/r/alpine/git)

+ Install

```
docker pull alpine/git
```

+ Execute

```
docker run -ti --rm -v %cd%:/root -v $(pwd):/git alpine/git <git_command>           // Windows
docker run -ti --rm -v ${PWD}:/root -v $(pwd):/git alpine/git <git_command>         // Linux
```

## Demo

[Python Machine Learning - Second Edition, published by Packt](https://github.com/PacktPublishing/Python-Machine-Learning-Second-Edition)
