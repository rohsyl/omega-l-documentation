[Retour](../classes.md)

# La classe `MailHelper`

Cette classe permet d'envoyé des mails.

`MailHelper` utilise la librairie [PHPMailer](https://github.com/PHPMailer/PHPMailer).

Si un serveur SMTP est configuré dans les Paramètres (onglet SMTP) du CMS, il va être utilisé. Sinon la fonction `mail()` est utilisée.

- Un exemple simple
    ```
    $from = array();
    $from[] = MailHelper::GetFromAddress(); // adresse mail
    $from[] = $from[0]; // nom de la personne
    
    $to = array();
    $to[] = 'jean@paul.pizza';
    
    $subject = 'Sujet du mail'
    $body = 'Contenu du mail';
    
    $result = MailHelper::SendMail(
        $from,
        null,
        $to,
        $subject,
        $body,
        strip_tags($body)
    );
    ```
    
- Ajouter l'option `replyTo`
    > Cette option permet à la personne qui va recevoir le mail de répondre à une autre adresse que celle qui lui a envoyé l'email.
    ```
    $from = array();
    $from[] = MailHelper::GetFromAddress(); // adresse mail
    $from[] = $from[0]; // nom de la personne
    
    $replyTo = array();
    $replyTo[] = 'syzin12@gmail.com'; // adresse mail
    $replyTo[] = 'Sylvain Roh'; // nom de la personne
    
    $to = array();
    $to[] = 'jean@paul.pizza';
    
    $subject = 'Sujet du mail'
    $body = 'Contenu du mail';
    
    $result = MailHelper::SendMail(
        $from,
        $replyTo,
        $to,
        $subject,
        $body,
        strip_tags($body)
    );
    ```
    
- [D'autres options de PHPMailer](https://github.com/PHPMailer/PHPMailer/wiki) qui ne sont pas expliquées ici ?

    Utiliser ce code pour avoir une instance de PHPMailer:
    ```
    $mail = MailHelper::InstancePHPMailer();
    ```
    
    Et si vous voulez utiliser le serveur SMTP configuré dans les paramètres (onglet SMTP) de OmegaCMS, vous pouvez y accéder grâce aux méthodes suivantes:
    
    Méthode | Description
    --- | ---
    `MailHelper::IsSmtpConfigured()` | Retourne `true` si un serveur SMTP est configuré, sinon retourne `false`
    `MailHelper::GetSmtpHost()` | Retourne l'adresse du serveur SMTP
    `MailHelper::GetSmtpPort()` | Retourne le port du serveur SMTP
    `MailHelper::GetSmtpHasAuth()` | Retourne `true` si le serveur SMTP nécessite une authentification, sinon retourne `false`
    `MailHelper::GetSmtpUsername()` | Retourne le `username` pour l'authentification
    `MailHelper::GetSmtpPassword()` | Retourne le `password` pour l'authentification
    `MailHelper::GetSecureMode()` | Si l'envoi de mail est sécurisé ou non
    
    Et les utiliser de la manière suivante :
    ```
    $mail = MailHelper::InstancePHPMailer();
    
    if(MailHelper::IsSmtpConfigured())
    {
        $mail->isSMTP();                                        // Set mailer to use SMTP
        $mail->Host 	= MailHelper::GetSmtpHost();  	    // Specify main and backup SMTP servers
        $mail->SMTPAuth 	= MailHelper::GetSmtpHasAuth();     // Enable SMTP authentication
        $mail->Username 	= MailHelper::GetSmtpUsername();    // SMTP username
        $mail->Password 	= MailHelper::GetSmtpPassword();    // SMTP password
        $mail->SMTPSecure 	= MailHelper::GetSecureMode();      // Enable TLS encryption, `ssl` also accepted
        $mail->Port 	= MailHelper::GetSmtpPort();        // TCP port to connect to
    }
    
    ...
    ```
    
- Envoyer un mail avec le serveur SMTP de Google
    > Configurer ces options dans les Paramètres (onglet SMTP) de OmegaCMS
    - Adresse : `smtp.gmail.com`
    - Port : `587`
    - Is TLS : `Oui`
    - Username : `username@gmail.com`
    - Password : `yourpassword`