**Note:** Hey! If you found this useful, leave a star so I know all of this was of use to someone! Thanks!

---

- Update 3: fixed local CS orientation issue, added example generated in WB, i used it to verify (thermal and mechanical parts both) the welding model with [this article](https://www.sciencedirect.com/science/article/abs/pii/S0927025607003345), it gives a good agreements with it, tho there's no mechanical part in the example script, you'll have to do it yourself if you want a full verification
- Update 2: added some missing commands
- Update 1: there was an error in the code on the init commit

# ANSYS-APDL-Welding-Script
So I decided to publish this old script of mine for weld simulation in APDL using Goldak heat source. This is a very bare-bones implementation, largely untested, but as far as I can remember it did the job. I have to note that I have a far more advanced version of a welding script with a lot of features but, well, it's a mess, an unholy abomination of spaghetti code nobody but I can make a sense of (tho I still struggle since it's been a while since I touched it). So it would be counter-productive to publish it, it would only confuse people, not helping with anything. This simple script is not intended as a ready solution but rather an example to build upon for your own purpose, it doesn't have all the features, all the little optimizations I figured out to squeeze out that extra millisecond of perfomance, but I hope it might help you get started. The model itself was verified with result from an article and gave me a pretty good agreement with it. If you have any questions - just open a new issue, I'll try to respond.

## Some helpful links:
- [apdl syntax for Notepad++](https://gist.github.com/sikvelsigma/9a4e602451a2c54986f9adba68f76320) (based on some syntax i found but i added a lot of keywords)
- [reddit thread](https://www.reddit.com/r/ANSYS/comments/jujsyq/help_with_apdl_code_for_goldak_heat_source/) where i posted some comments with advice about welding simulation
- [temperature-dependant material properties of mild steel](https://www.sciencedirect.com/science/article/abs/pii/S0927025607003345) i used for some simulations
- [temperature-dependant material properties of Ti-6Al-4V](https://repositories.lib.utexas.edu/handle/2152/89257)
- [temperature-dependant multi-linear kinematic hardening properties](https://www.sciencedirect.com/science/article/abs/pii/S0950061817301769) if you intend to do mechanical simulation
- [temperature-dependant film coefficient](https://www.sciencedirect.com/science/article/abs/pii/S0927025607003345)
- [a tool for digitizing graphs](https://automeris.io/WebPlotDigitizer/)
- use sci-hub (or one of its mirrors) to download papers, i won't link it here but it's not hard to find
## Some extra points:
- the script can be used with Workbench, just add apdl code snippet, set simulation time to greater than the one script produces to avoid an error and setup all neccessery selections
- to make a mechanical simulation with this you would probably need to use `ldread` command to read thermal data directly from `rth` file since i don't remember making it work in Workbench natively
- if you import data with `ldread` for mechanical solution you must divide your cooling times into smaller segments, you can't just import the last step - it will give incorrect solution since the process is non-linear
- do as much calculations as possible outside of loops, it significantly improves performance 
- when selecting stuff with commands you should test which order of selection works faster (remember: every millisecond counts since welding simulation is a very heavy task with lots of steps)
- you should probably save apdl `db` file each step for debug purposes (`SAVE` command) even if you work in Workbench
- you can use a `table` to apply loads, generally it's more efficient
- if you work with a `table` in apdl you can set each value as it's an array with 0-th elements (useful to setup axis)
- if you try to get a value for 0-th element in `table` (where axis coordinates are stored) you will always get 0 (this bug caused me so much trouble before i figured it out)
- you can mimic functions in apdl with `*create`, `*ulib` and `*use` commands which let you create marco files on the fly 
- you should delete all old macro files each run with `/delete` since apdl won't overwrite old files
- commands like `/go` and `/com` are useful for debug purposes and to track the solution process

## Last points
It is extremely unlikely I will update this anytime in the future since it's just an old script I used to figure out how to do stuff. Nevertheless, if someone for some reason wants to contribute - you're welcome to do so, but don't expect me to verify anything, I'm not doing jobs in this field of study anymore and I'm very short on time nowadays.
