Current Gunbot version: v21.1.0
- Official Stable Releases: [HERE](https://github.com/GuntharDeNiro/BTCT/releases/tag/2110)


ALPHA - first release - protoype

Sets up gunbot (with systemctl to run in the background) and includes chrony to keep time

Requirements to run - host needs to supply SYS_TIME permissions (working to look for this permission and ignore and workaround if the host is unable to supply these (restart the container every hour or so)

Default port: 5000 -> expose to host to access can be any port you want

example command runs a de-tached container that starts everything docker run -d --cap-add=SYS_TIME -p 5000:5000local/gunbot

Advanced users: add bash/sh to override startup script docker run -d --cap-add=SYS_TIME -p 5000:5000local/gunbot bash

Don't forget to start chrony service and gunbot service when ready systemctl enable --now chrony systemctl enable --now gunbot

Submit issues/feedback/feature requests at the linked GitHub site

File locations: /opt/gunbot -> gunbot stuff /var/log/journal/gunbot.service -> gunbot console log /tmp/startup.sh (auto executes using default command above)

Roadmap +mounting volume support is coming soon to then look for data files and take action from there -v "/full/path/host/gunbot/files:/opt/gunbot" -> right now clears the folder also will need to de-couple gunbot processes from data

#eventually add checks for failure of chrony or gunbot and then report back
#https://docs.docker.com/config/containers/multi-service_container/

#dynamically find gunthy-linux name for service in case it changes

#not working yet -> redirect logs to opt/gunbot ? right now in /var/log/journal/gunbot.service the console logs go here (outside standard logs that go to /opt/gunbot folder)

&& printf "StandardOutput=file:/opt/${GUNBOTDIR}/srvout.log\n" >> ${GUNBOTSERVICEFILE} \
&& printf "StandardError=file:/opt/${GUNBOTDIR}/srverr.log\n" >> ${GUNBOTSERVICEFILE} \
#eventually consider a healthcheck for the service and flag possibly chroncy service (for advanced users this would fail until chronyc service starts - would require SYS_TIME) #HEALTHCHECK --interval=60s --timeout=5s \

CMD chronyc tracking || exit 1
adjust startup script to provide further checks #provide script to enable service chrony and gunbot by default, if parameters provided script would be overrided for advanced users

connect other add-ins from marketplace either in a container or other containers (further review)

Add more distros (alpine / windows / macos support)

Check for beta releases and add a beta channel
