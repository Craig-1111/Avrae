embed <drac2>

# various shortcuts and determining a few baselines. What section of the alias they want to use, and whether they are able to use arcane or natural recovery. 

args = &ARGS&
ch = character() 
W = get("WizardLevel",0)
sc = load_json(get("subclass", "{}"))
D = sc.get("DruidLevel") == "Land"
L = get("DruidLevel",0)
mode = 1 if ('&1&'.lower()=='arcane' or '&1&'.lower()=='a') else 2 if ('&1&'.lower()=='natural' or '&1&'.lower()=='n') else 4 if ('&1&'.lower()=='help' or '&1&'.lower()=='?') else 5 if ('&1&'.lower()=='default' or '&1&'.lower()=='d') else 0
argsText = "&*&".strip("arcnetul")
n = '\n' 
out = []

# 3 options for this wizard, druid, or wizard with grimiore. While we are here, might as well define which cc is going to be used.

ch.set_cvar_nx("RecoverySettings",'{"grimoire": "false" ,"default": "low"}')
settingDefault = load_json(ch.get_cvar("RecoverySettings"))['default']
settingsGrimoire = load_json(ch.get_cvar("RecoverySettings"))['grimoire']
aGrim = 1 if settingsGrimoire == "true" else 0
aGrimText = "not" if (aGrim == 0) else ""
cc = "Arcane Recovery" if W else "Natural Recovery" if D else "" 
recSlotsMax = (ceil(int(W) / 2) + 1) if (aGrim == 1) else ceil(int(W or L) / 2)
ccDesc = "studying your spellbook." if W else "by sitting in meditation and communing with nature."

# manually selecting wizard  

if (mode == 1 or (mode == 0 and ('&2&'.lower()=='a' or '&2&'.lower()=='arcane'))) and W: 

    cc = "Arcane Recovery"
    recSlotsMax = (ceil(int(W) / 2) + 1) if aGrim == 1 else ceil(int(W) / 2)

# Druid

elif (mode == 2 or (mode == 0 and ('&2&'.lower()=='n' or '&2&'.lower()=='natural'))) and D:

    cc = "Natural Recovery"
    recSlotsMax = ceil(int(L) / 2)
    ccDesc = "by sitting in meditation and communing with nature."

# check to see if they can use this 

v = ch.cc_exists(cc) and ch.get_cc(cc)

# setting up the default selection. We'll do small as the starting point, I think. Less typing. 

if mode == 5: 

    # Break this down into 3 options, high default, low default, and instruction/status screen 

    if ("&2&".lower() == "high" or "&2&".lower() == "h"):

        # create a cvar to track this as the new default

        ch.set_cvar("RecoverySettings",'{"grimoire": "' + settingsGrimoire + '" ,"default": "high"}')

        # Make it look nice 

        out.append(f"-title 《{name} set their default recovery to High!》")
        out.append(f"-desc 《When you use `!recovery` it will automatically recover spell slots, starting with the highest available.》")
        out.append(f''' -thumb "{image}" ''')

    # change back to low spell slots

    elif ("&2&".lower() == "low" or "&2&".lower() == "l"):

        # remove the cvar, prevent clutter

        ch.set_cvar("RecoverySettings",'{"grimoire": "' + settingsGrimoire + '" ,"default": "low"}')
        out.append(f"-title 《{name} set their default recovery to Low!》")
        out.append(f"-desc 《When you use `!recovery` it will automatically recover spell slots, starting with the lowest available.》")
        out.append(f''' -thumb "{image}" ''')

    else: 

        # status and help screen 

        out.append(f"-title 《{name} could use some help with their Default Settings!》")
        out.append(f"-desc 《This is used to determine how `!recovery` automatically recovers spell slots. Currently, it will start with the {'highest' if settingDefault == 'high' else 'lowest'} Spell Slot{'s' if recSlotsMax != 1 else ''} available. \n\n**h|high** \nSets the alias default to automatically recover the highest level Spell Slot{'s' if recSlotsMax != 1 else ''} first. \n• `!recovery d h` \n\n**l|low** \nSets the alias default to automatically recover the lowest level Spell Slot{'s' if recSlotsMax != 1 else ''} first. \n• `!recovery d l`》")
        out.append(f''' -thumb "{image}" ''')

# General help screen 

elif mode == 4:
    cvarText = '{"DruidLevel": "Land"}'
    out.append(f"-title 《{name} could use some help!》")
    out.append(f"-desc 《An alias to more easily automate recovering spell slots with the Arcane and Natural Recovery features. \n\n**recovery** \nThe default command. Used by itself will automatically recover spell slots starting with the {'highest' if settingDefault == 'high' else 'lowest'} level available. Can be used with various arguments. \n\nIf you are a Land Druid, you will need to set your subclass through the `!level Druid Land` command in Derixyleth's [Verbose Character Tools](https://avrae.io/dashboard/workshop/5f7385fe647bb0a416316d1d) or do it manually with `!cvar subclass {cvarText}`. \n\n**#** \nNumbers can be added to only recover those specific Spell Slots. \n• `!recovery [#]` \n• eg `!recovery 1 1 2 5` \n\n**h|high** \nStarts with the highest level Spell Slots first when automatically recovering Spell Slots, despite what the default setting is. \n• `!recovery h` \n\n**l|low** \nStarts with the lowest level Spell Slots first when automatically recovering Spell Slots, despite what the default setting is. \n• `!recovery l` \n\n**d|default** \nUsed for changing your default setting for recovering Spell Slots, either highest or lowest first. Can be used to with `h|high` or `l|low` to change the default to high or low, respectively. \n• `!recovery d` \n• eg `!recovery d h` \n•*Currently set to recover the {'highest' if settingDefault == 'high' else 'lowest'} available Spell Slots first.* \n\n**g|grimoire** \nUsed for attuning to or removing an Arcane Grimoire. Can be used to with `a|attune` or `r|remove` to attune to or remove an Arcane Grimore, respectively. \n• `!recovery g` \n• eg `!recovery g a` \n•*Currently {'' if settingsGrimoire == 'true' else 'not'} attuned to an Arcane Grimoire.* \n\n**a|arcane** \nForces a use of Arcane Recovery. Only applicable when a character has both Arcane and Natural Recovery. \n• `!recovery a` \n• eg `!recovery a h` \n\n**n|natural** \nForces a use of Natural Recovery. Only applicable when a character has both Arcane and Natural Recovery. \n• `!recovery n` \n• eg `!recovery n 1 2 4`》")
    out.append(f''' -thumb {image} ''')

# Manually using Recovery 

elif ('&1&'.isdigit() or '&2&'.isdigit())  and v:

    # checking all the inputted spell slots 

    recSlotsRequested = [int(x) for x in args if x.isdigit() and int(x)<6]
    recSlotsRequestedTotal =sum(recSlotsRequested)

    # if that is possible 

    if recSlotsRequestedTotal <= recSlotsMax:
        for x in recSlotsRequested:
            ch.spellbook.set_slots(int(x), ch.spellbook.get_slots(int(x))+1)
        ch.set_cc(cc, 0)
        for x in range(1,10):
            out.append(f"{ch.spellbook.slots_str(x)}{(' (+' + recSlotsRequested.count(x) + ')') if recSlotsRequested.count(x) else ''}" if ch.spellbook.get_max_slots(x) else '')
        return f''' -title 《{name} uses their {cc} to Recover Spell Slots!》 -desc "You have learned to regain some of your magical energy by {ccDesc}. Once per day when you finish a Short Rest, you can choose expended spell slots to recover. The spell slots can have a combined level that is equal to or less than {recSlotsMax}, and none of the slots can be 6th level or higher." -f "{cc}|{ch.cc_str(cc)} (-1)" -f "Spell Slots|{n.join(out)}" -thumb "{"https://cdn.discordapp.com/attachments/945150617490456631/1020924055295103006/unknown.png" if cc == "Arcane Recovery" else "https://cdn.discordapp.com/attachments/945150617490456631/1020924094792867910/unknown.png" if cc == "Natural Recovery" else image}" '''
    
    # too many slots entered

    else: 
        out.append(f''' -title 《{name} fails to Recover Spell Slots!》''')
        out.append(f''' -desc "The number of slots exceeds the capacity of your {cc}."''')
        out.append(f''' -thumb "{"https://cdn.discordapp.com/attachments/945150617490456631/1020924055295103006/unknown.png" if cc == "Arcane Recovery" else "https://cdn.discordapp.com/attachments/945150617490456631/1020924094792867910/unknown.png" if cc == "Natural Recovery" else image}" ''')

    # they do not manually select some shit, cause lazy af like me 

elif v and any(ch.spellbook.get_slots(x) < ch.spellbook.get_max_slots(x) for x in range(1,recSlotsMax + 1)): 

    # set up some basic stuff, then do either high or low, depending on if they forced a certain command and what their default setting is 

    ch.set_cc(cc, 0)
    spellText = "You have learned to regain some of your magical energy by " + ccDesc + " Once per day when you finish a Short Rest, you can choose expended spell slots to recover. The spell slots can have a combined level that is equal to or less than " + recSlotsMax + ", and none of the slots can be 6th level or higher."
    recSlotsRequested = 0
    defaultMode = 1 if settingDefault == "high" else []
    forceHigh = 1 if "&1&".lower() == "h" or "&2&".lower() == "h" or "&1&".lower() == "high" or "&1&".lower() == "high" else 0
    forceLow = 1 if "&1&".lower() == "l" or "&2&".lower() == "l" or "&1&".lower() == "low" or "&1&".lower() == "low" else 0
    slotsVariable = "["

    # if starting with high

    if (defaultMode == 1 and forceLow != 1) or forceHigh == 1:
        while recSlotsRequested <= recSlotsMax:
            if ((recSlotsMax - recSlotsRequested) >= 5) and ch.spellbook.get_slots(5) < ch.spellbook.get_max_slots(5):
                recSlotsUsed = 5 
            elif ((recSlotsMax - recSlotsRequested) >= 4) and ch.spellbook.get_slots(4) < ch.spellbook.get_max_slots(4):
                recSlotsUsed = 4 
            elif ((recSlotsMax - recSlotsRequested) >= 3) and ch.spellbook.get_slots(3) < ch.spellbook.get_max_slots(3):
                recSlotsUsed = 3
            elif ((recSlotsMax - recSlotsRequested) >= 2) and ch.spellbook.get_slots(2) < ch.spellbook.get_max_slots(2):
                recSlotsUsed = 2 
            elif ((recSlotsMax - recSlotsRequested) >= 1) and ch.spellbook.get_slots(1) < ch.spellbook.get_max_slots(1):
                recSlotsUsed = 1
            else: 
                break
            ch.spellbook.set_slots(int(recSlotsUsed), ch.spellbook.get_slots(int(recSlotsUsed))+1)
            recSlotsRequested = recSlotsRequested + recSlotsUsed
            slotsVariable = slotsVariable + recSlotsUsed + (", " if ((recSlotsMax - recSlotsRequested != 0) or any(ch.spellbook.get_slots(x) < ch.spellbook.get_max_slots(x) for x in range(1,recSlotsMax + 1))) else "]")

        # Spell slot display

        for x in range(1,10):
            out.append(f"{ch.spellbook.slots_str(x)}{(' (+' + slotsVariable.count(str(x)) + ')') if slotsVariable.count(str(x)) else ''}" if ch.spellbook.get_max_slots(x) else '')
        slots = n.join(out)

        # put it all into an embed

        return f''' -title "{name} uses their {cc} to Recover Spell Slots!" -desc "{spellText}" -f "{cc}|{ch.cc_str(cc)} (-1)" -f "Spell Slots|{slots}" -thumb "{"https://cdn.discordapp.com/attachments/945150617490456631/1020924055295103006/unknown.png" if cc == "Arcane Recovery" else "https://cdn.discordapp.com/attachments/945150617490456631/1020924094792867910/unknown.png" if cc == "Natural Recovery" else image}"'''

    # if low 

    else:
        while recSlotsRequested <= recSlotsMax:
            if ((recSlotsMax - recSlotsRequested) >= 1) and ch.spellbook.get_slots(1) < ch.spellbook.get_max_slots(1):
                recSlotsUsed = 1 
            elif ((recSlotsMax - recSlotsRequested) >= 2) and ch.spellbook.get_slots(2) < ch.spellbook.get_max_slots(2):
                recSlotsUsed = 2 
            elif ((recSlotsMax - recSlotsRequested) >= 3) and ch.spellbook.get_slots(3) < ch.spellbook.get_max_slots(3):
                recSlotsUsed = 3
            elif ((recSlotsMax - recSlotsRequested) >= 4) and ch.spellbook.get_slots(4) < ch.spellbook.get_max_slots(4):
                recSlotsUsed = 4 
            elif ((recSlotsMax - recSlotsRequested) >= 5) and ch.spellbook.get_slots(5) < ch.spellbook.get_max_slots(5):
                recSlotsUsed = 5
            else: 
                break
            ch.spellbook.set_slots(int(recSlotsUsed), ch.spellbook.get_slots(int(recSlotsUsed))+1)
            recSlotsRequested = recSlotsRequested + recSlotsUsed
            slotsVariable = slotsVariable + recSlotsUsed + (", " if ((recSlotsMax - recSlotsRequested != 0) or any(ch.spellbook.get_slots(x) < ch.spellbook.get_max_slots(x) for x in range(1,recSlotsMax + 1))) else "]")

        # Spell slot display

        for x in range(1,10):
            out.append(f"{ch.spellbook.slots_str(x)}{(' (+' + slotsVariable.count(str(x)) + ')') if slotsVariable.count(str(x)) else ''}" if ch.spellbook.get_max_slots(x) else '')
        slots = n.join(out)

        # put it all into an embed

        return f''' -title "{name} uses their {cc} to Recover Spell Slots!" -desc "{spellText}" -f "{cc}|{ch.cc_str(cc)} (-1)" -f "Spell Slots|{slots}" -thumb "{"https://cdn.discordapp.com/attachments/945150617490456631/1020924055295103006/unknown.png" if cc == "Arcane Recovery" else "https://cdn.discordapp.com/attachments/945150617490456631/1020924094792867910/unknown.png" if cc == "Natural Recovery" else image}"'''

# cannot use 

else:      
    out.append(f''' -title 《{name} fails to Recover Spell Slots!》''')
    out.append(f''' -desc "You must complete a Long Rest to regain use of this feature, have spell slots available to recover, or have this as a class feature. See `!recovery ?` for more information."''')
    out.append(f''' -f "{cc}|{ch.cc_str(cc)}"''')
    out.append(f''' -thumb "{"https://cdn.discordapp.com/attachments/945150617490456631/1020924055295103006/unknown.png" if cc == "Arcane Recovery" else "https://cdn.discordapp.com/attachments/945150617490456631/1020924094792867910/unknown.png" if cc == "Natural Recovery" else image}" ''')

return n.join(out)
</drac2>
-footer "@Craig#1111    ||    !recovery ? | !recovery g | !recovery d"
-color <color>

