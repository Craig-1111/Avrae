embed <drac2>

# This is only split up for license reasons. Also, some variables 

args = &ARGS&
ch = character() 
W = get("WizardLevel",0)
n = '\n' 
out = []

# 3 options for this wizard, druid, or wizard with grimiore. While we are here, might as well define which cc is going to be used.

ch.set_cvar_nx("RecoverySettings",'{"grimoire": "false" ,"default": "low"}')
settingDefault = load_json(ch.get_cvar("RecoverySettings"))['default']
settingsGrimoire = load_json(ch.get_cvar("RecoverySettings"))['grimoire']
aGrim = 1 if settingsGrimoire == "true" else 0
aGrimText = "not" if (aGrim == 0) else ""

# setting up the Grimoire

# Break this down into 3 options, equip it, remove it, and instruction/status screen 

if ("&1&".lower() == "attune" or "&1&".lower() == "a") and W: 

    # create a cvar to track it being equiped

    ch.set_cvar("RecoverySettings",'{"grimoire": "true" ,"default": "' + settingDefault + '"}')

    # Make it look nice 

    out.append(f"-title 《{name} attunes to an Arcane Grimoire!》")
    out.append(f"-desc 《When you use your Arcane Recovery feature, you can increase the number of spell slot levels you regain by 1.》")

elif ("&1&".lower() == "remove" or "&1&".lower() == "r") and W:

    # remove the cvar, prevent clutter

    ch.set_cvar("RecoverySettings",'{"grimoire": "false" ,"default": "' + settingDefault + '"}')
    out.append(f"-title 《{name} removes an Arcane Grimoire!》")
    out.append(f"-desc 《When you use your Arcane Recovery feature, you can increase the number of spell slot levels you regain by 1.》")

else: 

    # status and help screen 

    out.append(f"-title 《{name} could use some help with their Arcane Grimoire!》")
    out.append(f"-desc 《While a wizard is attuned to an Arcane Grimoire and uses the Arcane Recovery feature, they can increase the number of spell slot levels they regain by 1. \n\nAn Arcane Grimoire is {aGrimText} currently attuned to. \n\n**a|attune** \nUsed to attune to an Arcane Grimoire. \n• `!recovery g a` \n\n**r|remove** \nUsed to remove an Arcane Grimoire. \n• `!recovery g r`》")

out.append(f''' -thumb "https://cdn.discordapp.com/attachments/945150617490456631/1020929134509170708/unknown.png" ''')

return n.join(out)

</drac2>
-footer "@Craig#1111    ||    !recovery ? | !recovery g | !recovery d"
-color <color>
