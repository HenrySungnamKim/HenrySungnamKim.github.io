---
layout: post
title:  "React Movie Informations Crawler"
date:   2018-08-06
excerpt: "react 영화 정보 크롤러 "
tag:
- 프로젝트
- react
project: true
comments: true
---
# React Movie Informations Crawler

## React로 작성된 Web-App입니다.

프로젝트 라이브 링크 : [https://henrysungnamkim.github.io/movie-app/](https://henrysungnamkim.github.io/movie-app/)
프로젝트 깃허브 링크 : [https://github.com/HenrySungnamKim/movie-app](https://github.com/HenrySungnamKim/movie-app)

<img width="1395" alt="screenshot2018-08-06at3-25fd4fb8-8a2c-46e1-8bcc-496688f05109 46 17pm" src="https://user-images.githubusercontent.com/37807838/43701182-38205318-9990-11e8-98d4-954159276816.png">

FaceBook create-react-app 모듈로 생성된 프로젝트.

사전에 필요한 프로그램 : yarn, npm, node, create-react-app

## 맥 OS에서 환경 설정하기

Brew를 이용해 설치하는 것이 편하다. [https://brew.sh/](https://brew.sh/)

Brew 설치

터미널에서
```
    /usr/bin/ruby -e "$(curl -fsSL [https://raw.githubusercontent.com/Homebrew/install/master/install](https://raw.githubusercontent.com/Homebrew/install/master/install))"
```
node 설치
```
    brew install node
```
node 버전확인
```
    node - v
```
npm 버전 확인
```
    npm -v
```
yarn 설치
```
    brew install yarn --without-node
```
yarn 버전 확인
```
    yarn -v
```
페이스북 react 모듈 create-react-app 다운로드
```
    npm install -g create-react-app
```
(폴더를 생성하고 싶은 위치로 이동.) 리액트 프로젝트 생성
```
    create-react-app movie-app
```
만약, npm WARN checkPermissions Missing write access to /usr/local/lib/node_modules 같은 오류메시지가 발생한다면
```
    sudo chown -R $USER /usr/local

```
폴더로 이동. 프로젝트 실행
```
    cd movie-app
    yarn start
```
http://locathost:3000/ URL로 로컬 서버가 실행된다.

## 무료 영화 API를 사용해서 JSON파일 받아오기
```javascript
    _callApi =() =>{
    return fetch('https://yts.am/api/v2/list_movies.json?sort_by=rating')
    .then(response=>response.json())
    .then(json=> json.data.movies)
    .catch(err=>console.log(err))
    }
```
## 비동기식 파일 교환
```javascript
    _getMovies = async ()=>{
        const movies = await this._callApi()
        this.setState({
          movies
        })
      }
```
