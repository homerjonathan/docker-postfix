Image providing [Postfix](http://www.postfix.org/) as a service.

You should make sure you mount spool volume (`/var/spool/postfix`) so that you do not
lose e-mail data when you are recreating a container. If a volume is empty, image
will initialize it at the first startup.

By default it is configured for sending outbound e-mails. Otherwise, you can extend
the image and configure it differently. See these examples:
 * [cloyne/postfix](https://github.com/cloyne/docker-postfix), which extends it to integrate
   it with [tozd/sympa](https://github.com/tozd/docker-sympa) mailing lists service
 * [tozd/mail](https://github.com/tozd/docker-mail), which extends it to provide a full-fledged
   e-mail service with virtual users

Remember that for the best e-mail delivery external IP should match the hostname it resolves to.

If you are extending this image, you can add two scripts which will be run at a container startup:
 * `/etc/service/postfix/run.config` – to prepare any custom configuration, before anything else is run
 * `/etc/service/postfix/run.initialization` – will be run after the container is initialized, but before the
   Postfix daemon is run

Used https://upcloud.com/community/tutorials/secure-postfix-using-lets-encrypt/ to assist in configuring.


docker build docker-postfix --file docker-postfix/alpine-38.dockerfile  --name jas1 -t jasoftware/docker-postfix-dovecot:alpine

docker run -it jasoftware/docker-postfix-dovecot:alpine -name jas1

docker exec -it "jas1" sh

docker push jasoftware/docker-postfix-dovecot:alpine


jasoftware/docker-postfix-dovecot:alpine
25 465 587
MAILNAME mail.jasoftware.com
ROOT_ALIAS jhomer@jasoftware.com
SMTPD_TLS_CERT_FILE /certificates/production/certs/jasoftware.com/fullchain.pem
SMTPD_TLS_KEY_FILE /certificates/production/certs/jasoftware.com/privkey.pem

/mail/log/postfix:/var/log/postfix
/mail/spool/postfix:/var/spool/postfix
/certificates/production/certs/jasoftware.com/:/certificates

# Portainer 

docker run -d -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock portainer/portainer