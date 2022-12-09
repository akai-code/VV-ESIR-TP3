# Balanced strings

A string containing grouping symbols `{}[]()` is said to be balanced if every open symbol `{[(` has a matching closed symbol `]}` and the substrings before, after and between each pair of symbols is also balanced. The empty string is considered as balanced.

For example: `{[][]}({})` is balanced, while `][`, `([)]`, `{`, `{(}{}` are not.

Implement the following method:

```java
public static boolean isBalanced(String str) {
    ...
}
```

`isBalanced` returns `true` if `str` is balanced according to the rules explained above. Otherwise, it returns `false`.

Use the coverage criteria studied in classes as follows:

1. Use input space partitioning to design an initial set of inputs. Explain below the characteristics and partition blocks you identified.
2. Evaluate the statement coverage of the test cases designed in the previous step. If needed, add new test cases to increase the coverage. Describe below what you did in this step.
3. If you have in your code any predicate that uses more than two boolean operators check if the test cases written so far satisfy *Base Choice Coverage*. If needed add new test cases. Describe below how you evaluated the logic coverage and the new test cases you added.
4. Use PIT to evaluate the test suite you have so far. Describe below the mutation score and the live mutants. Add new test cases or refactor the existing ones to achieve a high mutation score.

Write below the actions you took on each step and the results you obtained.
Use the project in [tp3-balanced-strings](../code/tp3-balanced-strings) to complete this exercise.

## Answer

Partitionnement de l'ensemble d'entree et generation de données de test:
l'ensemble des combinaison de symbole (;);{;};];[; est infini donc il faut partionner cet ensemble pour extraire un ensemble fini de cet ensemble qui permet d'entendre le comportement de notre fonction à l'ensemble infini des combinaisons de l'ensemble des symboles;
pour y arriver nous allons partitionner  l'ensemble E des combinaison des symboles en en 5 ensembles comme suite:
 
Pn l'emsemble des symboles de la forme {(...)} contenant n symboles et qui est balanced;
soit p2={([])};{[()]};[({})];[{()}];({[]});([{}]);
 
Qn la concatenation deux elements de pn aleatoire un exemple de Q2 est {([])}([{}]);
 
Tn  l'ensemble des elements de la forme Qn avec au moins un symbole inversé ; un exemple de T2 est :{([])}([{}](;
 
Fn une combinaison aleatoire de n symboles quelconque;
 
 #pour les generation des données de test on procedera comme suite:
 	* on choisie un nombre n ; 
 	*on genere les ensembles Pn;Qn;Tn;Fn;
 	le test doit etre true sur les ensemble Pn,Qn, et false sur l'emsemble Tn .
 	sur l'ensemble Fn on verifira si la chaine est des Pn ou Qn alors le test doit etre true sinon le test doit etre false.
