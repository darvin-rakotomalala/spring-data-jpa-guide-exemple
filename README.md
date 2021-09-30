## Spring Boot Data JPA et annotation @Query
Dans ce depo nous allons voir un petit guide de Spring Data JPA, examiner l'annotation `@Query` et comment l'utiliser dans les applications basées sur Spring.

### Qu'est-ce que Spring Data JPA ?
---
Spring Data JPA fait partie de la famille Spring Data.<br/>
Tout d'abord, ce framework s'appuie sur le framework Spring populaire et puissant et 
est considéré comme l'un des projets principaux de la suite d'outils de Spring.<br/>

Spring Data JPA s'appuie également sur et améliore JPA, qui signifie "Java Persistence API". 
La plupart des applications sont soutenues par une sorte de magasin de données. Au fur et à mesure 
que la complexité de votre application et l'ensemble des fonctionnalités augmentent, vous constaterez 
que votre couche d'accès aux données et votre code de niveau de persistance augmenteront également.<br/>

L'un des principaux objectifs de Spring Data JPA est de réduire votre code et de simplifier 
votre couche d'accès aux données, tout en conservant un ensemble de fonctionnalités riche et complet. 
Pour rendre cela possible, Spring Data JPA vous permet de créer des interfaces stéréotypées intelligentes Spring Repository.<br/>

Ces référentiels sont des interfaces Java qui vous permettent en tant que développeur de définir un contrat 
d'accès aux données. Le framework Spring Data JPA peut ensuite inspecter ce contrat et créer automatiquement 
l'implémentation de l'interface sous les couvertures pour vous.

### Fonctionnalités de JpaRepository
---
Lorsque vous étendez l'interface du référentiel JPA, vous avez également accès à de nombreuses autres 
fonctionnalités. La fonctionnalité fournie avec le référentiel JPA inclut les opérations CRUD.
**Fonctionnalité** :
* Interroger DSL, est un acronyme pour Domain Specific Language. C'est un terme utilisé pour classer une extension d'un langage de programmation pour adresser un domaine.
* Opérations CRUD
* Pagination et tri
* Aides
	* compter()
	* existe (identifiant long)
	* affleurer()
	* deleteInBatch(Entités itérables)

Il existe également des capacités de pagination et de tri, et enfin, le référentiel 
JPA contient quelques aides qui peuvent faciliter le travail avec votre couche d'accès aux données.

### Syntaxe de la méthode de requête
---
Premièrement, les méthodes de requête sont simplement des méthodes définies dans votre référentiel 
JPA que Spring Data JPA implémentera automatiquement en votre nom. C'est un moyen par lequel Spring Data JPA 
peut implémenter des requêtes pour vous.<br/>

* And	--> findByLastnameAndFirstname --> ...where x.lastname = ?1 and x.firstname = ?2
* Or --> findByLastnameOrFirstname	--> ...where x.lastname = ?1 or x.firstname = ?2
* Is, Equals	--> findByFirstnameEquals	--> ...where x.firstname = ?1
* Between	--> findByStartDateBetween --> ...where x.startDate between ?1 and ?
* LessThan --> findByAgeLessThan	--> ...where x.age < ?1
* LessThanEqual --> findByAgeLessThanEqual	--> ...where x.age <= ?1
* GreaterThan	--> findByAgeGreaterThan	--> ...where x.age > ?1
* GreaterThanEqual -> findByAgeGreaterThanEqual	- ...where x.age >= ?1
* After --> findByStartDateAfter	--> ...where x.startDate > ?1
* Before --> findByStartDateBefore	--> ...where x.startDate < ?1
* IsNull --> findByAgeIsNull	--> ...where x.age is null
* IsNotNull, NotNull - findByAge(Is)NotNull	- ...where x.age not null
* Like --> findByFirstnameLike	--> ...where x.firstname like ?1
* NotLike --> findByFirstnameNotLike	--> ...where x.firstname not like ?1
* StartingWith --> findByFirstnameStartingWith	--> ...where x.firstname like ?1 (parameter bound with appended %)
* EndingWith --> findByFirstnameEndingWith	--> ...where x.firstname like ?1 (parameter bound with prepended %)
* Containing --> findByFirstnameContaining	--> ...where x.firstname like ?1 (parameter bound wrapped in %)
* OrderBy --> findByAgeOrderByLastnameDesc	--> ...where x.age = ?1 order by x.lastname desc
* Not --> findByLastnameNot	--> ...where x.lastname <> ?1
* In --> findByAgeIn(Collection ages)	--> ...where x.age in ?1
* NotIn --> findByAgeNotIn(Collection ages)	--> ...where x.age not in ?1
* True --> findByActiveTrue()	--> ...where x.active = true
* False --> findByActiveFalse()	--> ...where x.active = false
* IgnoreCase --> findByFirstnameIgnoreCase	--> ...where UPPER(x.firstame) = UPPER(?1)

## Guide de l'annotation @Query
Dans Spring Data JPA on peur utiliser les méthodes de requête dérivées :
```
public interface MyRepository extends JpaRepository<Client, Long> {
   List<Client> findByOrganizationName(String name);
}
```
Ils constituent un moyen astucieux et rapide de se décharger du fardeau de l'écriture 
de requêtes sur Spring Data JPA en définissant simplement des noms de méthode.<br/>

Cet exemple, Spring Data JPA génère automatiquement une requête en fonction du nom de la méthode. 
Il va générer une requête qui renvoie une liste de tous les `Entity` enregistrements, avec un `organizationName`.<br/>

*Il s'agit d'une fonctionnalité extrêmement flexible et puissante de Spring Data JPA et elle vous permet d' amorcer des requêtes sans écrire les requêtes elles-mêmes, ni même implémenter une logique de gestion dans le back-end.*<br/>

Cependant, ils deviennent très difficiles à créer lorsque des requêtes complexes sont requises.<br/>

C'est à ce moment-là que vous préférerez probablement écrire vos propres requêtes. C'est faisable via l'annotation `@Query`.<br/>

*L'annotation `@Query` est appliquée au niveau de la méthode dans les interfaces `JpaRepository` et se rapporte à une seule méthode. Le langage utilisé à l'intérieur de l'annotation dépend de votre back-end, ou vous pouvez utiliser le JPQL neutre (pour les bases de données relationnelles).*<br/>

Lorsque la méthode est appelée, la requête à partir de l'annotation `@Query` se déclenche et renvoie les résultats.<br/>

### Qu'est-ce que JPQL ?
---
JPQL signifie Java Persistence Query Language . Il est défini dans la spécification JPA 
et est un langage de requête orienté objet utilisé pour effectuer des opérations de base de données sur des entités persistantes.<br/>

***Remarque** : Il convient de noter que par rapport au SQL natif, JPQL n'interagit pas avec les tables, 
les enregistrements et les champs de la base de données, mais avec les classes et les instances Java.*<br/>

Certaines de ses caractéristiques incluent :<br/>
* C'est un langage de requête indépendant de la plate-forme.
* C'est simple et robuste.
* Il peut être utilisé avec tout type de base de données relationnelle.
* Il peut être déclaré statiquement dans les métadonnées ou peut également être intégré dynamiquement dans le code.
* Il est insensible à la casse.

### Comprendre l'annotation @Query
---
L'annotation `@Query` ne peut être utilisée que pour annoter les méthodes d'interface du référentiel. 
L'appel des méthodes annotées déclenchera l'exécution de l'instruction qui s'y trouve, et leur utilisation est assez simple.<br/>

L'annotation `@Query` prend en charge à la fois le SQL natif et le JPQL. Lorsque le SQL natif est utilisé, 
le paramètre `nativeQuery`  de l'annotation doit être défini sur `true` :<br/>

Pour sélectionner tous les clients d'une base de données, nous pouvons utiliser soit une requête native soit JPQL :
```
@Query("SELECT(*) FROM CLIENT", nativeQuery=true)
List<Client> findAll();

@Query("SELECT client FROM Client client")
List<Client> findAll();
```
Cela se déclenche @Query lorsque vous appelez la méthode findAll().

### Paramètres de méthode de référencement
---
Les requêtes natives et les requêtes JPQL dans l'annotation `@Query` peuvent accepter des paramètres de méthode annotés, qui sont ensuite classés en :<br/>
* Paramètres basés sur la position
* Paramètres nommés

Lorsque vous utilisez des paramètres basés sur la position , vous devez garder une trace 
de l'ordre dans lequel vous fournissez les paramètres dans :<br/>
```
@Query("SELECT c FROM Client c WHERE c.name = ?1 AND c.age = ?2")
List<Client> findAll(String name, int age);
```
Le premier paramètre passé à la méthode est mappé sur `?1`, le second est mappé sur `?2`, etc.<br/>

D'autre part, les paramètres nommés sont bien nommés et peuvent être référencés par leur nom, quelle que soit leur position :<br/>
```
@Query("SELECT c FROM Client c WHERE c.name = :name and c.age = :age")
List<Client> findByName(@Param("name") String name, @Param("age") int age);
```
Le nom dans l'annotation `@Param` correspond aux paramètres nommés dans l'annotation `@Query`, 
vous êtes donc libre d'appeler vos variables comme vous le souhaitez - mais par souci de cohérence - il est conseillé d'utiliser le même nom.

### Conclusion
---
Les méthodes de requête dérivées sont une excellente fonctionnalité d'accélération des requêtes de 
Spring Data JPA, mais la simplicité se fait au détriment de l'évolutivité. Bien que flexibles, 
ils ne sont pas idéaux pour évoluer vers des requêtes complexes.<br/>
C'est là qu'intervient l'annotation `@Query` de Spring Data JPA.
