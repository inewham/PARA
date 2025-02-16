#Klipper Backup


Update package repos and install Git:
```
sudo apt update && sudo apt install git -y
```
Setup the repository for the backup files:
- Log in to GitHub.
- Within the **Repository** section click **New**.
- Within the **Create a new repository** section, enter a name into the **repository name** textbox.
- Click **Create repository**.
- Click on your profile image and select **settings**.
- Select **Developer settings** from the left hand menu.
- Click on **Personal access tokens** and select **Tokens (classic)**.
- Click on **Generate new token**.
- Select **Generate new token (classic)**.
- Enter a name in the **Note** textbox.
- Change the **Expiration** drop down to **No expiration**.
- Under **Select scope** select **repo** **write:packages** and **delete:packages** tick boxes.
- Click the **Generate token** button.
- Copy the displayed token.

Download install and configure Klipper-backup:
```
curl -fsSL get.klipperbackup.xyz | bash
~/klipper-backup/install.sh
```
Follow the installation wizard to config package.

Paste the below code into the relevant macro configuration file, this will put a button in the macro section on the dashboard:
```
[gcode_macro update_git]
gcode:
    {% set message = params.MESSAGE|default() %}
    {% if message %}
        RUN_SHELL_COMMAND CMD=update_git_script_message PARAMS="'{params.MESSAGE}'"
    {% else %}
        RUN_SHELL_COMMAND CMD=update_git_script
    {% endif %}

[gcode_shell_command update_git_script]
command: bash -c "bash $HOME/klipper-backup/script.sh"
timeout: 90.0
verbose: True

[gcode_shell_command update_git_script_message]
command: bash -c "bash $HOME/klipper-backup/script.sh -c \"$0\""
timeout: 90.0
verbose: True
```

##Reference

https://klipperbackup.xyz/
