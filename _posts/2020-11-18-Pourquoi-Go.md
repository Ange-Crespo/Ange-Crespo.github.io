---
layout: post
title: "Pourquoi choisir Go en 2020/2021 ?"
date: 2020-11-18
lang: fr
ref : goisthebest
categories: Techno
comments: true
author : colin
---

> Cet article technique s'adresse à des développeurs. 

__*Une remarque ? Des questions ? Laissez un commentaire !*__

## 1) Introduction&nbsp;:

&nbsp;&nbsp;&nbsp;&nbsp; Lorsqu'un développeur doit choisir un langage de programmation pour un projet, il est évident qu'il privilégiera les technologies par préférence et par expertise. Si le projet est important, d'autres point doivent également rentrer en ligne de compte : est-ce qu'il sera possible de trouver d'autres développeurs si l'équipe venait à grossir ? Est-ce que ce langage me permet d'atteindre mes objectifs rapidement ? Etc. 

&nbsp;&nbsp;&nbsp;&nbsp; Malgré l'explosion du développement orienté cloud en micro-service et le nombre hallucinant de projets tous aussi cool les uns que les autres qui ont émergé ces dix dernières années, plusieurs langages de programmation ont tout de même réussi à se hisser au rang de quasi-norme en ce qui concerne la programmation back-end : Ruby, Python, Java, JavaScript et PHP. Même si ces langages font partie [des 10 langages les plus utilisés](https://blog.newrelic.com/technology/most-popular-programming-languages-of-2019/), un seul figure dans les 10 langages les plus aimés et 3 figures au top 10 des plus détestés ! 

&nbsp;&nbsp;&nbsp;&nbsp; Il y a plusieurs raisons à leur succès mais je ne les détaillerai pas ici, je vous conseille l'excellent [talk de Richard Feldman](https://www.youtube.com/watch?v=QyJZzq0v7Z4) si vous n'êtes pas fâché avec la langue de Shakespeare.      

&nbsp;&nbsp;&nbsp;&nbsp; Il y a tout de même d'irréductibles gaulois (bien qu'internationaux). Parmi eux, Rust, Erlang et notre héros du jour : Go. Souvent considéré comme underground, Go a pourtant tout pour plaire mais peine encore à convaincre, surtout en France et en Europe.

## 2) Go, un langage qu'il est bien pour développer&nbsp;:
&nbsp;&nbsp;&nbsp;&nbsp; Ne vous y trompez pas, les débats entre techs pour choisir un langage de programmation s'apparentent plus souvent à une guerre religieuse qu'à un réel débat constructif. Je vous promets quand même de rester le plus objectif possible dans la suite de cet article ... 

ou pas ...

### a) Simplicité & propreté&nbsp;:
&nbsp;&nbsp;&nbsp;&nbsp; Go a cette particularité d'avoir une suite de normes de développement strictes. Une variable pas utilisée ? Erreur de compilation. Une bibliothèque importée mais pas utilisée ? Erreur de compilation. Une boucle écrite sur une seule ligne ? Erreur de compilation. Une fonction est appelée mais sa valeur de retour n'est pas prise en compte ? Un warning, et j'en passe. Ces simples règles peuvent rendre un programme beaucoup plus propre, surtout chez les débutants qui intégreront alors plus facilement les normes de développement moderne.

&nbsp;&nbsp;&nbsp;&nbsp; De plus, dès sa conception, les pères de Go ont voulu garder en tête la simplicité de sa mise en oeuvre. Par exemple, le mot-clé `while` n'existe pas en Go, il est remplacé par le mot-clé `for` qui peut aussi accepter des booléens. Le bout de pseudo-code qui suit est complètement valide en Go&nbsp;:

```go
package main

func main() {
    for CONDITION {
    	// fait des trucs
    }
}
```

Les itérations sur tableau se font aussi avec le mot-clé `for`&nbsp;:
```go
package main

import "fmt"

func main() {
    tab := []int{9, 8, 7, 6, 5, 4, 3, 2, 1, 0}
    for index, number := range tab {
        fmt.Println("number at", index, "is equal to", number)
    }
}
```

Sans oublier les boucles `for` classique&nbsp;:
```go
package main

import "fmt"

func main() {
    for index := 0; index < 10; index++ {
        fmt.Println(index)
    }
}
```

&nbsp;&nbsp;&nbsp;&nbsp; On pourrait donc résumer une partie de la philosophie de Go en une phrase : __*"un outil pour les gouverner tous"*__. Go est simple à écrire pour que vous, développeur, puissiez vous concentrer sur la complexité de votre problème. La courbe d'apprentissage du langage est vraiment douce et courte. De plus, Go propose un tutoriel pas à pas complet [ici](https://go-tour-fr.appspot.com/welcome/1) où vous apprendrez à coder un web crawler avant même que vous ayez eu le temps de finir de lire le sommaire de la [JavaDoc](https://docs.oracle.com/javase/specs/jls/se8/html/index.html) (à comparer avec la [documentation de Go](https://golang.org/ref/spec))  


### b) Un langage moderne&nbsp;:
&nbsp;&nbsp;&nbsp;&nbsp; Go est un langage qui vit avec son temps et intègre nativement [un packet manager](https://blog.golang.org/using-go-modules), [un linter](https://github.com/golang/lint), [un formateur](https://golang.org/cmd/gofmt/), [une doc](https://godoc.org/golang.org/x/tools/cmd/godoc), etc. et ça, quel plaisir ! Besoin d'une bibliothèque OpenSource ? `go get github.com/auteur/lib` avec gestion des versions en prime. La creation d'un pipeline de CI/CD(*) n'a jamais était aussi simple&nbsp;: 
```bash
# création du dossier vendor
go mod vendor                                                    
# vérification des conventions de programmation
golint -set_exit_status ./...                                    
# lancement des tests unitaires sur l'intégralité du projet
go test -short ./...                                             
# lancement des tests data races
go test -race -short ./...                                       
# lancement des vérifications sur l'allocation mémoire
go test -msan -short ./...                                       
# vérifie le pourcentage de code testé
go test -covermode=count -coverprofile "project_cover.cov" ./... 
# transformation du fichier "coverage" en HTML
go tool cover -html=project_cover.cov -o coverage.html           
# compilation du projet
go build -o executable cmd/main.go                               
```

> (*)CI/CD (Continuous Integration / Continuous Deployment) : Technique de management de code qui permet d'avoir une validation et un déploiement de code en production sans intervention humaine  

Il n'y a plus qu'à mettre les commandes dans des builds stages, quel bonheur d'utilisation ...

### c) La programmation parallèle & rapidité&nbsp;:
&nbsp;&nbsp;&nbsp;&nbsp; Voilà le plus gros avantage de Go, la programmation parallèle n'a jamais été aussi simple et efficace ... Je vous mets un exemple de lancement de thread pour vous prouver à quel point c'est facile, attention, ça va aller très vite&nbsp;:

```go
package main

// fonctionLongueUne est une fonction qui met très longtemps à être calculée
func fonctionLongueUne() {
    // fait des trucs
}

// fonctionLongueDeux est une autre fonction qui met très longtemps à être calculée
func fonctionLongueDeux() {
    // fait d'autres trucs
}

func main() {
    go func() {
        // lancement d'une fonction anonyme qui fait des trucs dans un thread
    }()
    go fonctionLongueUne() // on lance fonctionLongueUne dans un thread
    fonctionLongueDeux()   // on lance fonctionLongueDeux dans le main thread
    // c'est tout.
}
``` 

&nbsp;&nbsp;&nbsp;&nbsp; Fini les recherche sur votre moteur de recherche préféré sur le multithreading de tel ou tel langage avec la peur au ventre. 

__*Point bonus : Golang utilisera 100% de votre processeur ce qui n'est pas le cas sur Python ou JS !*__

&nbsp;&nbsp;&nbsp;&nbsp; De plus Go est pensé pour la vitesse d'exécution, et mon Dieu, qu'il est rapide ! Utilisant l'entièreté de votre CPU, il est souvent en tête des benchmarks au côté de Rust et de c/c++, le rendant [deux fois plus performant que JavaScript (NodeJS)](https://www.toptal.com/back-end/server-side-io-performance-node-php-java-go).

> Java peut également avoir des performances similaires à condition de ne pas utiliser le Garbage Collector ni la JVM, autant dire que c'est plus vraiment du Java et que ces configurations ne seront jamais utilisées en production. 

### d) Un typage fort(*) & gestion mémoire&nbsp;:
&nbsp;&nbsp;&nbsp;&nbsp; Go utilise un typage fort ce qui rend le coding plus intuitif ce qui permet d'écrire du code auto-commenté, une bonne pratique "native" de plus (bien qu'avoir de bons outils ne garantit pas un produit final de qualité ...) ! Petite particularité, les pointeurs sont de retour ! Même si certain n'apprécie pas forcément le typage (mais surtout les pointeurs, n'est-ce pas ?), il permet à Go une gestion fine de la pile (stack) et du tas (heap). Grâce à ça, Go vient avec un garbage collector très orienté performance au prix d'une consommation RAM plus importante (plus de détails [ici](https://stackoverflow.com/questions/40318257/why-go-can-lower-gc-pauses-to-sub-1ms-and-jvm-has-not) et [ici](https://blog.golang.org/ismmkeynote)).

&nbsp;&nbsp;&nbsp;&nbsp; Grâce à ça, il est possible de lire des fichiers de plusieurs gigas et de les charger en mémoire en quelques secondes ([ici, 16 Go en ~25secs](https://medium.com/swlh/processing-16gb-file-in-seconds-go-lang-3982c235dfa2)).

> (*)Typage fort : obligation de donnée un type à une variable, par exemple `var number int` est une déclaration d'une variable de type nombre entier (`int`)

### e) Les executables Go&nbsp;:
&nbsp;&nbsp;&nbsp;&nbsp; Quand Go compile, il ne fait pas semblant : il intègre dans le binaire l'ensemble de ses bibliothèques. Grâce à ça, il est possible de créer des [images Docker fonctionnel ne pesant que quelques Mo](https://medium.com/@chemidy/create-the-smallest-and-secured-golang-docker-image-based-on-scratch-4752223b7324).

&nbsp;&nbsp;&nbsp;&nbsp; La cross compilation est également simplifiée grâce à des variables d'environnements sans logiciel tier, voyez plutôt : 
```shell script
env GOOS=windows GOARCH=amd64 go build -o app.exe cmd/app/main.go  # compilation pour Windows
env GOOS=android GOARCH=arm go build -o app.apk cmd/app/main.go    # compilation pour Android
``` 

&nbsp;&nbsp;&nbsp;&nbsp; La liste des OS et des architectures supportées est [plutôt longue](https://gist.github.com/asukakenji/f15ba7e588ac42795f421b48b8aede63)...

### f) Go est bien supporté&nbsp;:
&nbsp;&nbsp;&nbsp;&nbsp; Quoiqu'on pense de Google, les gars ont quand même de la ressource, ça tombe bien, une partie de leurs ingénieurs travaillent sur le langage Go à temps plein ... Ça a le mérite d'assurer sa pérennité.


## 3) En conclusion&nbsp;:
&nbsp;&nbsp;&nbsp;&nbsp; Guerre de religion mise à part, Go est un super langage intuitif et définitivement tourné vers l'avenir. Malheureusement, il manque de support en Europe et certains de ces outils ne sont pas encore tout à fait modulables à souhait, comme son garbage collector ou son gestionnaire de packet. De plus, Go manque cruellement d'un support graphique dans sa librairie standard.

&nbsp;&nbsp;&nbsp;&nbsp; Si vous avez l'occasion de le tester, donner lui une chance de vous séduire !

*TLDR; Go est un super langage de programmation performant, lisible et maintenable sans effort particulier. Il manque cependant d'un support graphique et de plus d'option de customisation, largement dû à son jeune âge* 


{:.author}
*Auteur : [Colin Bois](https://github.com/clnbs), <colin.bois@rocketmail.com>*