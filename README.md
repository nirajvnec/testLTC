dir %USERPROFILE%\.ssh\id_rsa_odyssey

type %USERPROFILE%\.ssh\id_rsa_odyssey.pub | ssh git@odyssey.apps.csintra.net "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"

ssh git@odyssey.apps.csintra.net "chmod 700 ~/.ssh && chmod 600 ~/.ssh/authorized_keys"

ssh -i %USERPROFILE%\.ssh\id_rsa_odyssey git@odyssey.apps.csintra.net
