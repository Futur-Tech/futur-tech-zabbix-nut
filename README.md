# Zabbix Template for monitoring UPS with NUT

Right now only pfSense is supported.

Compatible Zabbix 6.0

## PfSense Install

> You need to have NUT already successfully setup.

- [ ] Login to pfSense Web GUI
- [ ] Go to **Diagnostics** > **Command Prompt**
    - [ ] Download the script by running this command:
    ```bash
    [ -d "/root/scripts" ] || mkdir /root/scripts ; curl -o /root/scripts/pfsense-nut-ups.sh https://raw.githubusercontent.com/Futur-Tech/futur-tech-zabbix-nut/main/pfsense-nut-ups.sh ; chmod u+x /root/scripts/pfsense-nut-ups.sh
    ```
    > You can add this command to **Services** > **Shellcmd** in order to download the latest version of the script, each time you reboot or restore a config backup.

- [ ] Go to **Services** > **Zabbix Agent 5.0**
    - [ ] At the bottom, click on **Show Advanced Options**
    - [ ] Add the following code to the **User Parameters**:
    ```bash
    UserParameter=upsmon[*],/root/scripts/pfsense-nut-ups.sh $1 $2
    ```
- [ ] Go to **Diagnostics** > **Command Prompt**
    - [ ] Test your installation by running the following command:
    ```bash
    /root/scripts/pfsense-nut-ups.sh name_of_your_ups
    # This should return a text string with the relevant UPS data.
    
    zabbix_agentd -c /usr/local/etc/zabbix50/zabbix_agentd.conf -t upsmon[ups.discovery]
    # This should return the name of your UPS
    ```

- [ ] Import the XML template to Zabbix 
- [ ] Add the template to the pfSense host and LLD should do the rest.

## Credit
https://github.com/hieronymousch and https://github.com/jtl999 for their code.