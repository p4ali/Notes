Local dnsmasq configuration:
```
address=/atlas.dev/192.168.33.10
```

Output:
```
±  |master ✗| → script/sanity-check 
+ '[' -z '' ']'
+ ATLAS_IP=192.168.33.10
+ ATLAS_HOST=192.168.33.10
+ '[' -z '' ']'
+ PROJECT=atlas-sanity.atlas.dev
+ TRIGGER=sanity-auth-trigger.sh
++ git config --get user.email
+ USEREMAIL=ali@perforce.com
+ USERNAME=ali
+ PASSWORD=password
+ echo 'Creating a project'
Creating a project
+ curl -s --data-urlencode name=atlas-sanity.atlas.dev --data-urlencode username=ali@perforce.com --data-urlencode password=password http://192.168.33.10/api/v1/depot
{"depot_id":1,"name":"atlas-sanity.atlas.dev"}
+ curl -s http://192.168.33.10/api/v1/depots {"depots":[{"id":1,"name":"atlas-sanity.atlas.dev"}]}
++ curl -s http://192.168.33.10/api/v1/depots
++ sed 's/.*"id":\([0-9]*\).*/\1/'
+ ID=1
+ echo

+ echo 'Creating a trigger'
Creating a trigger
+ curl -s --data-urlencode name=sanity-auth-trigger.sh --data-urlencode 'shell_script=exit 0' --data-urlencode 'command=Mytrigger change-commit //... "$SCRIPT$"' http://192.168.33.10/api/v1/depot/1/trigger
+ curl -s http://192.168.33.10/api/v1/depot/1/triggers
{"triggers":[{"id":1,"name":"sanity-auth-trigger.sh","command":"Mytrigger change-commit //... \"$SCRIPT$\""}]}+ echo

++ resolveip -s atlas-sanity.atlas.dev
+ '[' 192.168.33.10 '!=' 192.168.33.10 ']'
+ p4 -Cutf8 -patlas-sanity.atlas.dev:1666 -uali -Ppassword info
User name: ali
Client name: saas
Client host: saas
Client unknown.
Current directory: /Users/ali/git/raymond/frontend/atlas
Peer address: 172.17.42.1:50507
Client address: 192.168.33.1
Server address: 511687205b18:1666
Server root: /srv/p4d
Server date: 2014/09/01 04:08:28 +0000 UTC
Server uptime: 00:00:13
Server version: P4D/LINUX26X86_64/2014.1/907894 (2014/08/20)
Broker address: db.atlas.dev:1666
Broker version: P4BROKER/LINUX26X86_64/2014.2.PREP-TEST_ONLY/889594
Server license: none
Case Handling: sensitive
+ p4 -Cutf8 -patlas-sanity.atlas.dev:1666 -uali -Ppassword users
ali <ali@atlas> (ali) accessed 2014/09/01
git-fusion-user <git-fusion-user@atlas> (Git Fusion) accessed 2014/09/01
perforce <perforce@atlas> (perforce) accessed 2014/09/01
unknown_git <unknown_git@atlas> (unknown_git) accessed 2014/09/01
+ p4 -Cutf8 -patlas-sanity.atlas.dev:1666 -uali -Ppassword protect -o
# Perforce Protections Specification.
#
#  Each line contains a protection mode, a group/user indicator, the
#  group/user name, client host id and a depot file path pattern.
#  A user gets the highest privilege granted on any line.
#
#  Mode:        The permission level or right being granted or denied.  Each
#               permission level includes all the permissions above it,
#               except for 'review'.  Each permission right only includes
#               the specific right and not all the lesser rights. Modes
#               preceded by '=' are rights; all other modes are levels.
#
#               list   - users can see names but not contents of files;
#                        users can see all non-file related metadata
#                        (clients, users, changelists, jobs, etc.)
#
#               read   - users can sync, diff, and print files
#
#               open   - users can add, edit, delete, and integrate files
#
#               write  - users can submit open files
#
#               admin  - permits those administrative commands and command
#                        options that don't affect the server's security
#
#               super  - allows access to the 'p4 protect' command
#
#               review - allows access to the 'p4 review' command; implies
#                        read access
#
#               =read  - if this right is denied, users cannot sync, diff,
#                        or print files
#
#               =branch - if this right is denied, users are not permitted
#                         to use files as a source for 'p4 integrate'
#
#               =open   - if this right is denied, users cannot open files
#                         (add, edit, delete, integrate)
#
#               =write  - if this right is denied, users cannot submit open
#                         files
#
#  Group/User indicator: either 'group' or 'user'.
#
#  Name:        A Perforce group or user name; may be wildcarded.
#
#  Host:        The IP address of a client host; may be wildcarded, or
#               may instead use CIDR syntax, e.g. 172.16.0.0/16 would match
#               all IPv4 addresses which start with 172.16.
#
#  Path:        The part of the depot being granted access.

Protections:
	write user * * //...
	super user perforce * //...
	super user ali * //...
	admin user git-fusion-user * //...
	review user git-fusion-reviews-* * //...

+ p4 -Cutf8 -patlas-sanity.atlas.dev:1666 -uali -Ppassword triggers -o
# Perforce Submit and Form Validating Trigger Specifications.
#
#  Triggers:	a list of triggers; one per line. Each trigger line must be
#		indented with spaces or tabs in the form. Each line has four
#		elements:
#
#  		Name:   The name of the trigger.
#
#  		Type:   'archive'	  external archive access triggers
#			'auth-check'      check authentication trigger
#			'auth-check-sso'  sso check authentication trigger
#			'auth-set'        set authentication trigger
#			'change-submit'   pre-submit triggers
#			'change-content'  modify content submit triggers
#			'change-commit'   post-submit triggers
#			'change-failed'   submit failure fires these triggers
#			'command'         pre/post user command triggers
#			'edge-submit'     Edge Server pre-submit
#			'edge-content'    Edge Server content submit
#			'fix-add'         pre-add fix triggers
#			'fix-delete'      pre-delete fix triggers
#			'form-in'         modify form in triggers
#			'form-out'        modify form out triggers
#			'form-save'       pre-save form triggers
#			'form-commit'     post-save form triggers
#			'form-delete'     pre-delete form triggers
#			'service-check'   check auth trigger (service users)
#			'shelve-submit'   pre-shelve triggers
#			'shelve-commit'   post-shelve triggers
#			'shelve-delete'   pre-delete shelve triggers
#
#  		Path:   For change-*, edge-*, or shelve-*triggers, a pattern
#			to match files in the changelist.
#
#			For form-* triggers, the type of form: e.g. 'branch'
#			'client', etc.
#
#			For fix-* triggers use 'fix'.
#
#			For auth-* triggers use 'auth'.
#
#			For archive triggers, a file pattern to match the
#			file name being accessed.
#
#			For command triggers, the client command to match.
#			Must be in the form "(pre|post)-user-$command",
#			e.g. "pre-user-tag".  The command name is a regular
#			expression.  See "p4 help grep" for details on
#			syntax.
#
#  		Command: The OS command to run for validation.  If the
#			 command contains spaces, the whole command must
#			 be quoted.  See 'p4 help triggers' for a list of
#			 variables that can be expanded in the command
#			 string.
#
#  For example,
#
#	Triggers:
#		cscheck change-submit //depot/... "cmd %changelist%"
#		no-oblits command pre-user-obliterate fail
#		mkspec form-out client "%quote%//trig/scr.pl%quote%"
#
# See 'p4 help triggers' for more information about triggers.

Triggers:
	GF-change-content change-content //... "/usr/bin/python /usr/local/git-fusion/bin/p4gf_submit_trigger.py change-content            %changelist% %user% %client% %serverport% %command% %args%"
	GF-change-commit change-commit //... "/usr/bin/python /usr/local/git-fusion/bin/p4gf_submit_trigger.py change-commit             %changelist% %user% %client% %serverport% %oldchangelist% %command% %args%"
	GF-change-failed change-failed //... "/usr/bin/python /usr/local/git-fusion/bin/p4gf_submit_trigger.py change-failed             %changelist% %user% %client% %serverport% %command% %args%"
	Atlas-Mytrigger change-commit //... %serverroot%/triggers/sanity-auth-trigger.sh

```
