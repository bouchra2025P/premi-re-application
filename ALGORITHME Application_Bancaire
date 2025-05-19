ALGORITHME Application_Bancaire

/* === CONSTANTES === */
CONSTANTE MAX_CLIENTS ← 100
CONSTANTE MAX_USERS ← 100
CONSTANTE MAX_TRANSACTIONS ← 10

/* === STRUCTURES === */

TYPE Transaction
    description : Chaine de caractères
    montant : Réel
Fin TYPE

TYPE Utilisateur
    identifiant : Chaine de caractères 
    motDePasse : Chaine de caractères
    solde : Réel
    TABLEAU historique[MAX_TRANSACTIONS] : Transaction
    nbTransactions : entier
Fin TYPE

/* === VARIABLES GLOBALES === */
TABLEAU utilisateurs[MAX_USERS] : Utilisateur
TABLEAU base_clients[MAX_CLIENTS][6] : Chaine de caractères
Variables 
choix, action, nb_utilisateurs, nb_clients : entier

/* === FONCTION verifier_identifiants === */
Fonction verifier_identifiants(user : Chaine de caractères , password : Chaine de caractères) : BOOLEEN
Variables 
i : entier
Début
    Pour i ← 0 à nb_utilisateurs - 1 Faire
        Si utilisateurs[i].identifiant = user ET utilisateurs[i].motDePasse = password Alors
            Retourne VRAI
        FinSi
    FinPour
    Retourne FAUX
Fin

/* === FONCTION verifier_code === */
Fonction verifier_code(code_saisi : Chaine de caractères, code_attendu : Chaine de caractères) : BOOLEEN
Début
    Si code_saisi = code_attendu Alors
        Retourne VRAI
    Sinon
        Retourne FAUX
    FinSi
Fin

/* === PROCEDURE changer_mot_de_passe === */
Procédure changer_mot_de_passe(identifiant, nouveau_mdp : Chaine de caractères)
Variables i : entier
Début
    Pour i ← 0 à nb_utilisateurs - 1 Faire
        Si utilisateurs[i].identifiant = identifiant Alors
            utilisateurs[i].motDePasse ← nouveau_mdp
        FinSi
    FinPour
Fin

/* === PROCEDURE envoyer_code_verification === */
Procédure envoyer_code_verification(cin, email : Chaine de caractères)
Variables 
i : entier
client_trouve : BOOLEEN
code_utilisateur, code_envoye, nouveau_mdp : Chaine de caractères
Début
    client_trouve ← FAUX
    Pour i ← 0 à nb_clients - 1 Faire
        Si base_clients[i][0] = cin ET base_clients[i][1] = email Alors
            client_trouve ← VRAI
        FinSi
    FinPour
    Si client_trouve = Vrai Alors
        Ecrire("Code envoyé à l'email.")
        code_envoye ← "123456"  /* Simulation de code envoyé*/
        Ecrire("Saisir le code envoyé :")
        Lire(code_utilisateur)
        Si verifier_code(code_utilisateur, code_envoye) Alors
            Ecrire("Saisir le nouveau mot de passe :")
            Lire(nouveau_mdp)
            changer_mot_de_passe(cin, nouveau_mdp)
            Ecrire("Mot de passe mis à jour")
        Sinon
            Ecrire("Code incorrect")
        FinSi
    Sinon
        Ecrire("CIN ou email incorrect")
    FinSi
Fin

/* === FONCTION authentifier === */
Fonction authentifier(identifiant, motDePasse : Chaine de caractères) : Entier
Variables i : entier
Début
    Pour i ← 0 à nb_utilisateurs - 1 Faire
        Si utilisateurs[i].identifiant = identifiant ET utilisateurs[i].motDePasse = motDePasse Alors
            Retourne i
        FinSi
    FinPour
    Retourne -1
Fin

/* === PROCEDURE ajouter_transaction === */
Procedure ajouter_transaction(u : REF Utilisateur, desc : Chaine de caractères, montant : Réel)
Début
    Si u.nbTransactions < MAX_TRANSACTIONS Alors
        u.historique[u.nbTransactions].description ← desc
        u.historique[u.nbTransactions].montant ← montant
        u.nbTransactions ← u.nbTransactions + 1
    FinSi
Fin

/* === PROCEDURE consulter_solde === */
Procedure consulter_solde(u : REF Utilisateur)
Début
    Ecrire("Votre Solde est : ", u.solde)
Fin

/* === PROCEDURE voir_transactions === */
Procedure voir_transactions(u : REF Utilisateur)
Variables i : entier
Début
    Si u.nbTransactions = 0 Alors
        Ecrire("Aucune transaction.")
    Sinon
        Pour i ← 0 à u.nbTransactions - 1 Faire
            Ecrire(u.historique[i].montant, " - ", u.historique[i].description)
        FinPour
    FinSi
Fin

/* === PROCEDURE faire_virement === */
Procedure faire_virement(u : REF Utilisateur)
Variables identifiant_destinataire : Chaine de caractères, montant : Réel, destinataireIndex, i : entier
Début
    Ecrire("Saisir l’identifiant du destinataire :")
    Lire(identifiant_destinataire)

    Ecrire("Saisir le montant que vous voulez envoyer :")
    Lire(montant)

    Si montant ≤ 0 OU montant > u.solde Alors
        Ecrire("Montant invalide ou solde insuffisant.")
        Retour
    FinSi

    destinataireIndex ← -1
    Pour i ← 0 à nb_utilisateurs - 1 Faire
        Si utilisateurs[i].identifiant = identifiant_destinataire Alors
            destinataireIndex ← i
        FinSi
    FinPour

    Si destinataireIndex = -1 Alors
        Ecrire("Destinataire introuvable.")
        Retour
    FinSi

    u.solde ← u.solde - montant
    utilisateurs[destinataireIndex].solde ← utilisateurs[destinataireIndex].solde + montant
    ajouter_transaction(u, "Virement vers " + identifiant_destinataire, -montant)
    ajouter_transaction(utilisateurs[destinataireIndex], "Virement reçu de " + u.identifiant, montant)
    Ecrire("Virement de ", montant, " MAD effectué vers ", identifiant_destinataire)
Fin

/* === PROCEDURE payer_facture === */
Procedure payer_facture(u : REF Utilisateur)
Variables type, reference : Chaine de caractères, montant : Réel
Début
    Ecrire("Entrez le type de facture :")
    Lire(type)
    Ecrire("Entrez la référence de la facture :")
    Lire(reference)
    Ecrire("Entrez le montant de la facture :")
    Lire(montant)

    Si montant ≤ 0 OU montant > u.solde Alors
        Ecrire("Montant invalide ou insuffisant")
        Retour
    FinSi

    u.solde ← u.solde - montant
    ajouter_transaction(u, "Paiement " + type + " (ref " + reference + ")", -montant)
    Ecrire("Paiement effectué")
Fin

/* === PROCEDURE recharger_mobile === */
Procédure recharger_mobile(u : REF Utilisateur)
Variables numero : Chaine de caractères
 montant : Réel
Début
    Ecrire("Entrez le numéro à recharger :")
    Lire(numero)
    Ecrire("Entrez le montant à recharger :")
    Lire(montant)

    Si montant ≤ 0 OU montant > u.solde Alors
        Ecrire("Montant invalide ou solde insuffisant.")
        Retour
    FinSi

    u.solde ← u.solde - montant
    ajouter_transaction(u, "Recharge mobile " + numero, -montant)
    Ecrire("Recharge de ", montant, " MAD envoyée à ", numero)
Fin

/* === PROGRAMME PRINCIPAL === */
Début
    /* Initialisation base utilisateurs */
utilisateurs[0].identifiant ← "mohamed"
    utilisateurs[0].motDePasse ← "1234"
    utilisateurs[0].solde ← 6234.34
    utilisateurs[0].nbTransactions ← 0

    utilisateurs[1].identifiant ← "asma"
    utilisateurs[1].motDePasse ← "abcd"
    utilisateurs[1].solde ← 3245.06
    utilisateurs[1].nbTransactions ← 0

    utilisateurs[2].identifiant ← "bouchra"
    utilisateurs[2].motDePasse ← "pass"
    utilisateurs[2].solde ← 15657.20
    utilisateurs[2].nbTransactions ← 0

    /* Initialiser base_clients[] avec quelques entrées (CIN + email) */
 base_clients[MAX_CLIENTS][6] ← [
    ["AA123",ali,daouidi,Casablanca,0657689902 ali@gmail.com ],
    ["SS456", Sara,Yasami,RABAT_Ville,0756712893,sara@yahoo.com]
]

    nb_utilisateurs ← 3
    nb_clients ← 2

estConnecte ← FAUX
    userIndex ← -1

    nb_utilisateurs ← 0
    nb_clients ← 0

    estConnecte ← FAUX
    userIndex ← -1
    /*  Authentification / création / récupération */

    TantQue estConnecte = FAUX Faire
        Ecrire("1 - Connexion / 2 - Créer un compte / 3 - Mot de passe oublié")
        Ecrire("Saisir votre choix :")
        Lire(choix)

        Selon choix Faire
            CAS 1:
                Ecrire("Entrer identifiant utilisateur :")
                Lire(identifiant)
                Ecrire("Entrer mot de passe :")
                Lire(motDePasse)
                userIndex ← authentifier(identifiant, motDePasse)
                Si userIndex ≠ -1 Alors
                    estConnecte ← VRAI
                    Ecrire("Connexion réussie")
                Sinon
                    Ecrire("Erreur d'identifiants ou de mot de passe")
                FinSi
            CAS 2:
                Si nb_clients < MAX_CLIENTS ET nb_utilisateurs < MAX_USERS Alors
                    Ecrire("Saisir votre CIN :")
                    Lire(cin)
                    Ecrire("Saisir votre nom:")
                    Lire(nom)
                    Ecrire("Saisir votre prenom :")
                   Lire(prenom)
                   Ecrire("Saisir votre adresse :")
                   Lire(adresse)
                   Ecrire("Saisir votre telephone : ")
                   Lire(telephone)
                   Ecrire("Saisir votre email : ")
                   Lire(email)

                    base_clients[nb_clients][0] ← cin
                    base_clients[nb_clients][1] ← nom
                    base_clients[nb_clients][2] ← prenom
                   base_clients[nb_clients][3] ← adresse
                   base_clients[nb_clients][4] ← telephone
                    base_clients[nb_clients][5] ← email
                    utilisateurs[nb_utilisateurs].identifiant ← cin
                    utilisateurs[nb_utilisateurs].motDePasse ← "0000"
                    utilisateurs[nb_utilisateurs].solde ← 0
                    utilisateurs[nb_utilisateurs].nbTransactions ← 0
                    nb_clients ← nb_clients + 1
                    nb_utilisateurs ← nb_utilisateurs + 1
                    Ecrire("Compte créé avec succès")
                Sinon
                    Ecrire("Limite de création atteinte")
                FinSi
            CAS 3:
                Ecrire("Saisir votre CIN :")
                Lire(cin)
                Ecrire("Saisir votre email :")
                Lire(email)
                envoyer_code_verification(cin, email)
            Autre:
                Ecrire("Choix invalide")
        FinSelon
    FinTantQue

  /* Menu bancaire */

    utilisateur_courant ← utilisateurs[userIndex]
    TantQue VRAI Faire
        Ecrire("1. Solde / 2. Transactions / 3. Virement / 4. Facture / 5. Recharge / 6. Quitter")
        Lire(action)
        Selon action Faire
            CAS 1: consulter_solde(utilisateur_courant)
            CAS 2: voir_transactions(utilisateur_courant)
            CAS 3: faire_virement(utilisateur_courant)
            CAS 4: payer_facture(utilisateur_courant)
            CAS 5: recharger_mobile(utilisateur_courant)
            CAS 6:
                Ecrire("Merci, au revoir")
                Quitter
            Autre:
                Ecrire("Choix invalide")
        FinSelon
    FinTantQue
Fin
