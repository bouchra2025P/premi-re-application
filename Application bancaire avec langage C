#include <stdio.h>
#include <string.h>
#include <stdbool.h>

#define MAX_CLIENTS 100
#define MAX_USERS 100
#define TAILLE 100
#define MAX_TRANSACTIONS 10

// === Structures ===
typedef struct {
    char description[100];
    float montant;
} Transaction;

typedef struct {
    char identifiant[20];
    char motDePasse[20];
    float solde;
    Transaction historique[MAX_TRANSACTIONS];
    int nbTransactions;
} Utilisateur;

// === Bases de données simulées ===
Utilisateur utilisateurs[MAX_USERS] = {
    {"mohamed", "1234", 6234.34, {{"Virement initial", 2000.00}}, 1},
    {"asma", "abcd", 3245.06, {}, 0},
    {"bouchra", "pass", 15657.20, {}, 0}
};
int nb_utilisateurs = 3;

char base_clients[MAX_CLIENTS][6][TAILLE] = {
    {"AA123","ali","daouidi","Casablanca","0657689902","ali@gmail.com"},
    {"SS456","Sara","Yasami","RABAT_VILLE","0756712893","sara@yahoo.com"}
};
int nb_clients = 2;

// === Fonctions Utilitaires ===
bool verifier_identifiants(const char* user, const char* password) {
    for (int i = 0; i < nb_utilisateurs; i++) {
        if (strcmp(user, utilisateurs[i].identifiant) == 0 &&
            strcmp(password, utilisateurs[i].motDePasse) == 0) {
            return true;
        }
    }
    return false;
}

bool verifier_code(const char* code) {
    return strcmp(code, "1234") == 0;
}

void changer_mot_de_passe(const char* identifiant, const char* nouveau_mdp) {
    for (int i = 0; i < nb_utilisateurs; i++) {
        if (strcmp(identifiant, utilisateurs[i].identifiant) == 0) {
            strcpy(utilisateurs[i].motDePasse, nouveau_mdp);
            return;
        }
    }
}

void envoyer_code_verification(const char* cin, const char* email) {
    bool client_trouve = false;
    char code_utilisateur[TAILLE], nouveau_mdp[TAILLE];

    for (int i = 0; i < nb_clients; i++) {
        if (strcmp(cin, base_clients[i][0]) == 0 &&
            strcmp(email, base_clients[i][1]) == 0) {
            client_trouve = true;
            break;
        }
    }

    if (client_trouve) {
        printf("Un code de vérification a été envoyé à votre email : %s\n", email);
        printf("Entrez le code reçu : ");
        scanf("%s", code_utilisateur);

        if (verifier_code(code_utilisateur)) {
            printf("Entrez le nouveau mot de passe : ");
            scanf("%s", nouveau_mdp);
            changer_mot_de_passe(cin, nouveau_mdp);
            printf("Mot de passe mis à jour avec succès.\n");
        } else {
            printf("Code incorrect.\n");
        }
    } else {
        printf("Erreur : CIN ou email incorrect\n");
    }
}

int authentifier(char* identifiant, char* motDePasse) {
    for (int i = 0; i < nb_utilisateurs; i++) {
        if (strcmp(utilisateurs[i].identifiant, identifiant) == 0 &&
            strcmp(utilisateurs[i].motDePasse, motDePasse) == 0) {
            return i;
        }
    }
    return -1;
}

void ajouter_transaction(Utilisateur *u, const char* desc, float montant) {
    if (u->nbTransactions < MAX_TRANSACTIONS) {
        strcpy(u->historique[u->nbTransactions].description, desc);
        u->historique[u->nbTransactions].montant = montant;
        u->nbTransactions++;
    }
}

void consulter_solde(Utilisateur *u) {
    printf("Votre solde actuel est de %.2f MAD\n", u->solde);
}

void voir_transactions(Utilisateur *u) {
    printf("=== Historique des transactions ===\n");
    if (u->nbTransactions == 0) {
        printf("Aucune transaction enregistrée.\n");
        return;
    }
    for (int i = 0; i < u->nbTransactions; i++) {
        printf("%.2f MAD - %s\n", u->historique[i].montant, u->historique[i].description);
    }
}

void faire_virement(Utilisateur *u) {
    char destinataire[50];
    float montant;

    printf("Entrez l'identifiant du destinataire : ");
    scanf("%s", destinataire);
    printf("Entrez le montant à transférer : ");
    scanf("%f", &montant);

    if (montant <= 0 || montant > u->solde) {
        printf("Montant invalide ou solde insuffisant.\n");
        return;
    }

    int destinataireIndex = authentifier(destinataire, utilisateurs[authentifier(u->identifiant, u->motDePasse)].motDePasse); // Authentifie uniquement par ID
    if (destinataireIndex == -1) {
        printf("Destinataire introuvable.\n");
        return;
    }

    u->solde -= montant;
    utilisateurs[destinataireIndex].solde += montant;

    char desc[100];
    sprintf(desc, "Virement vers %s", destinataire);
    ajouter_transaction(u, desc, -montant);

    sprintf(desc, "Virement reçu de %s", u->identifiant);
    ajouter_transaction(&utilisateurs[destinataireIndex], desc, montant);

    printf("Virement de %.2f MAD vers %s effectué.\n", montant, destinataire);
}

void payer_facture(Utilisateur *u) {
    char type[20], reference[30];
    float montant;

    printf("Entrez le type de facture : ");
    scanf("%s", type);
    printf("Entrez la référence : ");
    scanf("%s", reference);
    printf("Montant à payer : ");
    scanf("%f", &montant);

    if (montant <= 0 || montant > u->solde) {
        printf("Montant invalide ou solde insuffisant.\n");
        return;
    }

    u->solde -= montant;

    char desc[100];
    sprintf(desc, "Paiement %s (ref %s)", type, reference);
    ajouter_transaction(u, desc, -montant);

    printf("Paiement de %.2f MAD effectué.\n", montant);
}

void recharger_mobile(Utilisateur *u) {
    char numero[20];
    float montant;

    printf("Entrez le numéro à recharger : ");
    scanf("%s", numero);
    printf("Entrez le montant à recharger : ");
    scanf("%f", &montant);

    if (montant <= 0 || montant > u->solde) {
        printf("Montant invalide ou solde insuffisant.\n");
        return;
    }

    u->solde -= montant;

    char desc[100];
    sprintf(desc, "Recharge mobile %s", numero);
    ajouter_transaction(u, desc, -montant);

    printf("Recharge de %.2f MAD envoyée à %s\n", montant, numero);
}

// === Fonction principale ===
int main() {
    char choix[TAILLE];
    char user[TAILLE], password[TAILLE];
    char cin[TAILLE], nom[TAILLE], prenom[TAILLE], adresse[TAILLE];
    char telephone[TAILLE], email[TAILLE];
    bool estConnecte = false;
    int userIndex = -1;

    while (!estConnecte) {
        printf("\nChoisissez une action :\n");
        printf("1 - Connexion\n");
        printf("2 - Créer un compte\n");
        printf("3 - Mot de passe oublié\n");
        printf("Votre choix : ");
        scanf("%s", choix);

        if (strcmp(choix, "1") == 0) {
            printf("Identifiant utilisateur : ");
            scanf("%s", user);
            printf("Mot de passe : ");
            scanf("%s", password);

            userIndex = authentifier(user, password);
            if (userIndex != -1) {
                estConnecte = true;
                printf("Connexion réussie !\n");
            } else {
                printf("Identifiants incorrects.\n");
            }

        } else if (strcmp(choix, "2") == 0) {
            printf("Création de compte\n");
            printf("CIN : "); scanf("%s", cin);
            printf("Nom : "); scanf("%s", nom);
            printf("Prénom : "); scanf("%s", prenom);
            printf("Adresse : "); scanf("%s", adresse);
            printf("Téléphone : "); scanf("%s", telephone);
            printf("Email : "); scanf("%s", email);

            if (nb_clients < MAX_CLIENTS && nb_utilisateurs < MAX_USERS) {
                strcpy(base_clients[nb_clients][0], cin);
                strcpy(base_clients[nb_clients][1], nom);
                strcpy(base_clients[nb_clients][2], prenom);
                strcpy(base_clients[nb_clients][3], adresse);
                strcpy(base_clients[nb_clients][4], telephone);
                strcpy(base_clients[nb_clients][5], email);
                nb_clients++;

                strcpy(utilisateurs[nb_utilisateurs].identifiant, cin);
                strcpy(utilisateurs[nb_utilisateurs].motDePasse, "0000"); // mot de passe temporaire
                utilisateurs[nb_utilisateurs].solde = 0;
                utilisateurs[nb_utilisateurs].nbTransactions = 0;
                nb_utilisateurs++;

                printf("Compte créé avec succès. Mot de passe temporaire : 0000\n");
            } else {
                printf("Erreur : base de données pleine.\n");
            }

        } else if (strcmp(choix, "3") == 0) {
            printf("Mot de passe oublié\n");
            printf("CIN : ");
            scanf("%s", cin);
            printf("Email : ");
            scanf("%s", email);
            envoyer_code_verification(cin, email);
        } else {
            printf("Choix invalide.\n");
        }
    }

    // Menu bancaire
    Utilisateur *u = &utilisateurs[userIndex];
    int action, continuer = 1;

    while (continuer) {
        printf("\n--- Menu Principal ---\n");
        printf("1. Consulter solde\n");
        printf("2. Voir transactions\n");
        printf("3. Faire un virement\n");
        printf("4. Payer une facture\n");
        printf("5. Recharger mobile\n");
        printf("6. Quitter\n");
        printf("Votre choix : ");
        scanf("%d", &action);

        switch (action) {
            case 1: consulter_solde(u); break;
            case 2: voir_transactions(u); break;
            case 3: faire_virement(u); break;
            case 4: payer_facture(u); break;
            case 5: recharger_mobile(u); break;
            case 6: continuer = 0; printf("Merci. À bientôt !\n"); break;
            default: printf("Choix invalide.\n");
        }
    }

    return 0;
}
