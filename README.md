**Questions, bugs, or release news?** Join the [SerrebiProjects Telegram group](https://t.me/SerrebiProjects), the fastest place to get help.

# BlindTerm MUD directory

This repository holds the generated MUD directory downloaded by [BlindTerm](https://git.serrebiradio.com/serrebi/BlindTerm). It lives on the self-hosted Forgejo at git.serrebiradio.com.

The directory branch contains mud-directory.json, served to BlindTerm from https://git.serrebiradio.com/serrebi/BlindTerm-directory/raw/branch/directory/mud-directory.json. A Forgejo Actions job in BlindTerm (`.forgejo/workflows/mud-directory.yml`) rebuilds and force-pushes that branch twice an hour from MUDVerse, MUDStats, Grapevine, The Mud Connector, and GMUD. Its history is intentionally replaced on every refresh so generated data does not grow without bound.

Do not edit the generated branch by hand. Source code and issue tracking belong in the main BlindTerm repository.
