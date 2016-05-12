Minecraft Mod Repository
========================

This is a project to build a repository/package manager for Minecraft mods,
allowing players to easily modders to easily publish mods, players to easily
download mods, and modpack creators to easily, *ahem*, create modpacks. This
project is currently in its earliest stages. Once we get a rough idea of
exactly what we want in such a tool and what parts would be necessary, we will
get started on coding.


Wishlist
--------

These are the features we would like to have. For now, anything goes. Once the
list has pretty much settled down, we will seperate it into phases so we can
start implementation. Have an idea? Send a PR and we'll add it to this list.

* Easy mod installation
  * This should be as simple as searching on a mod's name and clicking install.
  * Dependencies should be automatically installed as well.
* Easy modpack assembly
  * Users should be able to easily distribute a list of mods along with any
    additional files (e.g. configs) for other players to install.
  * Versions should be kept identical, with a possible exception for versions
    which are marked as exclusively bug fixes.
  * Modpacks should be able to be forked and re-distributed easily.
* Optional accounts
  * The only accounts people should need are Mojang accounts. If they wish to
    share mods or modpacks, they may need a seperate account.
  * If possible, it would be good to emulate Minecraft's own auth flow in our
    client so that Mojang accounts could be the only accounts necessary at all.
* Compensation
  * To gain any traction among modders, we need to be able to compensate modders
    somehow. Ideas for how we could do this:
    * An option within the client where players could donate to an individual
      modder or make a donation to be equally spread among the authors of all
      mods in an instance.
    * *Light* ads in the client
* Lightweight client
  * During gameplay, the client should use the smallest possible amound of
    system resources while still being useful to allow more resources to be
    dedicated to the game. This probably means not keeping all game logs in
    memory and instead loading old logs automatically as the user scrolls up.
* CI service for modders
  * In the "winning modders over" department, we'll want to do something BETTER
    than the current leader, Curse, in order to succeed. A CI service would be
    one way to do that. We could automatically watch GH repositories for new
    tags, and when they appear automatically compile them and make the new
    release available for download. This means that, aside from a few minutes of
    initial setup, adding a mod to MMR would be a "set it and forget it" thing,
    meaning that adding a mod to MMR becomes less of a "why" and more of a "why
    not."
* Simple server setup
  * Allow a player to enter a server IP and automatically get an instance with
    that server's modpack and the server's IP preloaded into the server list.
* Single local mod repository
  * Forge allows us to store mods in a single local repository instead of in the
    mods folder of each instance. Let's use this to save on disk space.
* Resource pack support
  * Sort of like the FTB Launcher. It would be even better to independently
    store the core resource pack and any mod patches for each pack, allowing
    automatic generation of resource packs for brand new and/or small packs that
    wouldn't normally get resource packs put together.
* Cross-Platform
  * The client should work on Windows, OS X and Linux from Day 1.
* Customizable Modpacks
  * In addition to the ability to make arbritary customizations to a a pack, it
    would be good to have preconfigured options to allow players to customize.
    See ATLauncher.
* External Mods
  * Modpack developers should be able to include in their packs Mods that have a
       license that permits modpack inclusion, but whose authors have not
       uploaded them to our repository. These should proably be handled the same
       way Curse does: store them on our side as JARs, within the modpack along
       with configs.
* API
  * Anyone should be able to build a client or other software that uses our data.
* Permissions
  * Modders should be able to privately "release" their mods for a private beta,
    or publicly release them. If a mod is publicly released, there should be
    four options: "do not allow in modpacks," "allow in private modpacks",
    "allow in public modpacks," and "allow in monetized public modpacks."
    Obviously, even if this last option was selected both the modder and the
    modpacker would be paid. If the mod was still in a private beta, only the
    first two options would be available.
  * Modpack developers should be able to make a modpack private (whitelist of
    player names), or public.
    
Architecture
============

These are the big components I forsee needing.

Backend
-------

This is the system that hosts mod files along with mod metadata and modpacks.
The CI system will also be in this component.

**Exposed Interface:** JSON API
**Requirements:** Light on resources (disk space, bandwidth, CPU) so that it is
affordable.

Client Core
-----------

This handles storing mods locally, communicating with the backend, and
interfacing with Minecraft.

**Builds on:** Backend
**Exposed Interface:** Library
**Requirements:** Light on resourses (CPU, Memory) so that it can run alongside
Minecraft without slowing it down

Client User Interface
---------------------

This is what the user sees. It needs to allow building modpacks, launching
instances, etc.

**Builds on:** Client Core
**Exposed Interface:** CLI first, seperate GUI implementation later
**Requirements:** Easy to use, light on resources (see client core)

Web Interface
-------------

This is similar to Curse's CurseForge. It allows casual browsing of available
mods, and some of the things the client UI allows (everything but managing
instances).

**Builds on:** Backend
**Exposed Interface:** Website
**Requirements:** Simple to navigate
