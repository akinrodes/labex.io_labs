# LAB 1

In Unix-like operating systems, setuid (set user ID) is a special file permission that allows a user to execute a file with the permissions of the file's owner. While this can be useful for certain system operations, it can also pose security risks if misused. In this challenge, you'll learn how to identify and list all setuid files on a system, which is an essential skill for system administrators and security professionals.

[labex@host ~]$ sudo find / -type f -perm -4000 2>/dev/null > setuid_list

| Octal | Bit      | Nom          | Description                                                                                       |
| ----- | -------- | ------------ | ------------------------------------------------------------------------------------------------- |
| 4000  | `setuid` | Set User ID  | Le programme s’exécute avec les **droits du propriétaire**, pas de l’utilisateur courant.         |
| 2000  | `setgid` | Set Group ID | Le programme s’exécute avec les **droits du groupe** du fichier.                                  |
| 1000  | `sticky` | Sticky Bit   | Sur un dossier, empêche les utilisateurs de supprimer des fichiers qui ne leur appartiennent pas. |

| Bit spécial | Fichier exécutable         | Répertoire                       |
| ----------- | -------------------------- | -------------------------------- |
| `setuid`    | ✅ s'exécute avec UID owner | 🚫 Sans effet                    |
| `setgid`    | ✅ s'exécute avec GID group | ✅ héritage du groupe             |
| `sticky`    | 🚫 ignoré                  | ✅ empêche suppression par autres |

NB:

-Trouver les fichiers avec le setuid bit (4000)
    find / -type f -perm -4000 2>/dev/null
Ces fichiers s’exécutent avec les droits du propriétaire, souvent root.

-Trouver les fichiers avec le setgid bit (2000)
    find / -type f -perm -2000 2>/dev/null
Ces fichiers s’exécutent avec les droits du groupe du fichier.

-Trouver les répertoires avec le setgid bit (très utilisé sur les dossiers partagés)
    find / -type d -perm -2000 2>/dev/null
Les fichiers créés dans ce dossier héritent du groupe du dossier parent.

-Trouver les répertoires avec le sticky bit (1000)
    find / -type d -perm -1000 2>/dev/null

-Combinez les permissions
Pour chercher setuid et setgid en même temps :
  find / -type f \( -perm -4000 -o -perm -2000 \) 2>/dev/null
