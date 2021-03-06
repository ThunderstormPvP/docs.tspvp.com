---
layout: page

category: "Examples"
category_lead:  "XML File Examples"
title:  "Harb"

---

>If you're interested in the backstory of harb read: [The Story of Harb](/misc/harb) - By SH4D0W_HAWK

[<i class="fa fa-share right-ref-link"></i>](/modules/main)
Specify the XML file version and then open the main map module and specify the maps name, version and objective.

    <?xml version="1.0"?>

    <map proto="{{site.current_proto}}">
    <name>Harb</name>
    <version>1.3.5</version>
    <objective>Be the team with the least amount of deaths after 10 minutes.</objective>

<br/>
This map has 2 major authors specified and no contributors.

    <authors>
        <author uuid="1379cb6e-f291-4498-9807-e636f9674ac0"/> <!--  SH4D0W_HAWK  -->
        <author uuid="ef4ea031-998f-4ec9-b7b6-1bdd428bcef8"/> <!--  Plastix  -->
    </authors>

<br/>
[<i class="fa fa-share right-ref-link"></i>](/modules/teams)
Define two teams, their [colors](/reference/formatting#chatColors) and names.

    <teams>
        <team id="blue-team" color="blue" max="50">Blue Team</team>
        <team id="red-team" color="dark red" max="50">Red Team</team>
    </teams>

<br/>
[<i class="fa fa-share right-ref-link"></i>](/modules/filters)
Custom filter definition that denies TNT blocks. When applied to a region with `block=""` it denies placing and breaking. To only deny placing apply it with `block-place=""`.

    <filters>
        <deny id="no-tnt"><material>TNT</material></deny>
    </filters>

<br/>
[<i class="fa fa-share right-ref-link"></i>](/modules/regions)
Define several regions with ID's. Apply the `no-tnt` filter to the region `bases` and never allow block placing/breaking in the `portals-area` region.

    <regions>
        <rectangle id="main-area" min="-50,-32" max="51,33"/>
        <union id="bases">
            <rectangle id="blue-base" min="-20,-62" max="21,-32"/>
            <rectangle id="red-base" min="-20,33" max="21,63"/>
        </union>
        <complement id="portals-area">
            <rectangle min="-56,-2" max="57,3"/>
            <region id="main-area"/>
        </complement>

        <apply block="never" region="portals-area"/> <!-- protect portal areas -->
        <apply block="no-tnt" region="bases" message="You may not place TNT in the bases."/>
    </regions>

<br/>
[<i class="fa fa-share right-ref-link"></i>](/modules/kits)
Each team kit has leather armor colored in their team color and they inherit the main spawn kit.

    <kits>
        <kit id="spawn">
            <item slot="0" material="stone sword"/>
            <item slot="1" enchantment="arrow infinite:1" material="bow"/>
            <item slot="28" amount="1" material="arrow"/>
            <item slot="3" amount="64" material="cooked chicken"/>
            <item slot="2" amount="2" material="TNT"/>
            <item slot="5" amount="32" material="ladder"/>
            <item slot="4" amount="3" material="golden apple"/>
            <item slot="6" name="TNT Defuser" lore="`7Right click to defuse teammate's TNT" material="shears"/>
            <item slot="7" damage="8194" material="potion"/> <!-- potion of swiftness 1 -->
            <potion duration="5">heal</potion>
            <potion duration="10">damage resistance</potion>
        </kit>
        <kit id="red" parents="spawn">
            <helmet color="cd0000" unbreakable="true" material="leather helmet"/>
            <chestplate color="cd0000" enchantment="protection explosions:3" unbreakable="true" material="leather chestplate"/>
            <leggings color="cd0000" unbreakable="true" material="leather leggings"/>
            <boots color="cd0000" unbreakable="true" material="leather boots"/>
        </kit>
        <kit id="blue" parents="spawn">
            <helmet color="0066cc" unbreakable="true" material="leather helmet"/>
            <chestplate color="0066cc" enchantment="protection explosions:3" unbreakable="true" material="leather chestplate"/>
            <leggings color="0066cc" unbreakable="true" material="leather leggings"/>
            <boots color="0066cc" unbreakable="true" material="leather boots"/>
        </kit>
    </kits>

<br/>
[<i class="fa fa-share right-ref-link"></i>](/modules/spawns)
Harb has pretty unique spawns in that they are cuboids covering a large area of the map that transverse multiple Z levels. This allows players to spawn on roofs and inside buildings. Because of this `safe="true"` is specified to prevent players from spawning midair or in walls. The `<point>` definition is a failsafe in case the PGM plugin can't find a safe spawn inside the region.

    <spawns>
        <spawns safe="true" sequential="true">
            <spawn team="blue-team" kit="blue" yaw="0">
                <cuboid min="-15,32,-57" max="16,42,-32"/>
                <point>0.5,33,-47.5</point>
            </spawn>
            <spawn team="red-team" kit="red" yaw="180">
                <cuboid min="-15,32,33" max="16,42,58"/>
                <point>0.5,33,48.5</point>
            </spawn>
        </spawns>
        <default yaw="90"><cuboid min="-75.5,42.5,-0.5" max="-73.5,42.5,1.5"/></default>
    </spawns>

<br/>
[<i class="fa fa-share right-ref-link"></i>](/modules/portals)
These portals link two opposite sides of the map together. They move the players current position by 110 blocks so that they end up infront of the other portal.

    <portals>
        <portal x="110">
            <cuboid min="-56,33,-1" max="-55,35,2"/>
        </portal>
        <portal x="-110">
            <cuboid min="56,33,-1" max="57,35,2"/>
        </portal>
    </portals>

<br/>
[<i class="fa fa-share right-ref-link"></i>](/modules/gamemode_tdm)
Set the game type to team deathmatch and specify a max run time of 10 minutes.

    <score>
        <kills>1</kills>
        <deaths>1</deaths>
    </score>
    <time>10m</time>

<br/>
[<i class="fa fa-share right-ref-link"></i>](/modules/repair_remove_keep)
Repair stone swords, bows, and shears when dropped or when the player picks one up.

    <toolrepair>
        <tool>stone sword</tool>
        <tool>bow</tool>
        <tool>shears</tool>
    </toolrepair>

<br/>
[<i class="fa fa-share right-ref-link"></i>](/modules/repair_remove_keep)
Remove armor, arrows, ladders, cooked chicken, etc. To prevent the playing field and player inventories from getting cluttered with items.

    <itemremove>
        <item>leather helmet</item>
        <item>leather chestplate</item>
        <item>leather leggings</item>
        <item>leather boots</item>
        <item>arrow</item>
        <item>ladder</item>
        <item>cooked chicken</item>
        <item>glass bottle</item>
        <item>golden apple</item>
        <item>clay ball</item>
        <item>glowstone dust</item>
        <item>string</item>
    </itemremove>

[<i class="fa fa-share right-ref-link"></i>](/modules/killreward)
Give players a golden apple every time they kill an enemy.

    <killreward>
        <item material="golden apple"/>
    </killreward>

<br/>
[<i class="fa fa-share right-ref-link"></i>](/modules/tnt)
Instantly ignite TNT when placed and disable TNT explosions from breaking blocks.

    <tnt>
        <instantignite>on</instantignite>
        <blockdamage>off</blockdamage>
    </tnt>

<br/>
[<i class="fa fa-share right-ref-link"></i>](/modules/disabledamage)
Disable damage from TNT placed by teammates.

    <disabledamage>
        <damage ally="true" self="false" enemy="false" other="false">block explosion</damage>
    </disabledamage>

<br/>
Close the main `<map>` module.

    </map>
