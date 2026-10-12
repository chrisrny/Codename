# Codename

Agent Ollama durci qui garde un `{codeword}` derrière un `{password}`, plus un
toolkit de 5 attaques par injection de prompt. Modèle imposé : `granite3.1-moe:3b`.

## Arborescence

```
Modelfile              l'agent (version courante)
versions/Modelfile_vN  historique des versions
attacks/*.prompt       les 5 attaques, une famille par fichier
run_tests.py           tests de la défense (sans interface chatbot)
```

## Build & run

```
ollama pull granite3.1-moe:3b
ollama create codename -f Modelfile
ollama run codename
```

Le correcteur remplace `{password}` et `{codeword}` par de vraies valeurs.

## Attaques (toolkit)

Une famille distincte par fichier :

| Fichier | Famille |
|---|---|
| `override.prompt` | surcharge d'instructions (fausse mise à jour prioritaire) |
| `persona.prompt` | jeu de rôle / fiction |
| `encoding.prompt` | encodage / obfuscation de la sortie |
| `indirect.prompt` | injection indirecte dans une tâche |
| `refusal_suppression.prompt` | suppression du refus via contrainte de format |

## Tests

```
python3 run_tests.py
```

Reconstruit l'agent avec de vraies valeurs et envoie chaque message directement
au modèle (pas de chatbot), puis affiche OK/FAIL par cas.

## Limites connues

- un mot de passe identique à la casse près (`tangerine42`) libère encore le codeword ;
- un mot de passe noyé dans une phrase anodine ne déclenche pas toujours la libération.
