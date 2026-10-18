# Windows Guide: How to Add Custom Music to Forza Horizon 1 (Pinyon Shift)

This guide details the step-by-step process for replacing the songs on the original Forza Horizon 1 radio stations running through the Pinyon Shift emulator, specifically using the **PinyonShift-Launcher**.

---

## ⚠️ Prerequisites

1. **Music in the correct format:** All audio files must be strictly converted to **MP3, 48 kHz, 160 kbps CBR (Constant Bitrate) and Stereo**. Other formats will cause errors or audio desync.
2. **Python:** Installed on your computer.
3. **FFmpeg:** Required for the script to process the audio (it must be installed or in the same folder as the script).
4. **Radio Tool:** The `build-fh1-radio.py` script, available in the `tools/` folder of the Pinyon Shift repository.
5. Official downloads: [Download Python](https://www.python.org/downloads/); [Download FFmpeg](https://www.gyan.dev/ffmpeg/builds/), scroll down to the release builds section and download the .zip or .7z file

---

## Step 1: Preparing the music folder

Create a folder on your computer and place all the MP3 songs you want to add to the radio in it. 
*Example:* `D:\Recomps\Customs\musics`

*(Note: The script will bundle all these files into a single `.fsb` package. To add more songs in the future, just place the new files in this same folder (`D:\Recomps\Customs\musics`) and run the process again).*

---

## Step 2: Running the Mod Generation Script

Open the terminal (Command Prompt or PowerShell), navigate to the folder where the `build-fh1-radio.py` script is located (usually in `\PinyonShift-Launcher\tools\`) and run the command below.

Pay attention to the arguments:
* `--station Radio2`: Replaces the Horizon Pulse station. (Use `Radio1` for Bass Arena or `Radio3` for Rocks).
* `--replace`: Removes the original playlist of the chosen station and plays *only* your songs.
* `--name my_musics`: Names the mod folder. **Tip:** Always use lowercase letters with no spaces or symbols. Example: `my_musics`.

**Example command:**
```cmd
python .\build-fh1-radio.py "D:\Recomps\Customs\musics" --station Radio2 --replace --name my_musics --enable --game-root "D:\Recomps\Decompilers\PinyonShift-Launcher\.local\game\base" --state-root "D:\Recomps\Decompilers\PinyonShift-Launcher\.local\state"
```
The script will process the songs and create the audio database and the mod structure.

---

## Step 3: Enabling the Mod in PinyonShift-Launcher (The Crucial Step)

If you use a custom launcher with "preview" versions, Pinyon Shift ignores the default `.local\state` settings folder and starts reading its settings from a dedicated folder called `.local\preview`

For the game to recognize your mod, you need to enable it in the correct configuration file:

1. Navigate to the active file's directory: `D:\Recomps\Decompilers\PinyonShift-Launcher\.local\preview\config\`
2. Open the `pinyon_shift.toml` file with a text editor (such as Notepad).
3. Find the `enabled_mods` line. It will probably already contain mods enabled by the launcher
4. Add your mod's name (`my_musics`) to the end of the list, separated by a comma and inside the quotes. It should look something like this:

```toml
enabled_mods = "zz-archive-patches,immersive_camera,my_musics"
```

5. Save and close the file.

(Organization Tip: During testing, it was found that the launcher's mods are usually stored in \mods_src\builtin. Although it is not strictly required if the .toml is configured correctly, you can move the `my_musics` folder generated in Step 2 (located in `.local\state\mods\`) into builtin to keep things organized in line with the launcher).

## Step 4: Avoiding the "Silent Radio Bug" (noRepeat Adjustment)

The game has an anti-repeat system on the radio that prevents the most recently played songs from repeating. If you added only a few songs (e.g. 9 songs) and the game tries not to repeat the last 10, the radio will go silent.

1. Navigate to the directory where your mod was saved and is being read by the game (either the `state\mods` folder or `mods_src\builtin`).
2. Follow the path to the generated XML file: `...\my_musics\media\audio\radio\RadioSystem.xml`
3. Open it in Notepad and search (`Ctrl + F`) for the name of the station you changed (e.g. `"Radio2"`).
4. Locate the playlist tag: `<Playlist noRepeat="X">`
5. Change the value of `"X"` to a number lower than the number of songs you added. (For example, if you added 9 songs, change it to `noRepeat="3"`).
6. Save the file.

## Step 5: Testing in Game

1. Open Forza Horizon 1 through the PinyonShift-Launcher.
2. On the home screen or pause menu, go to SETTINGS > MODS.
3. Check that your `MY_MUSICS` mod appears in the list as enabled.
4. Go back to the game, get in the car, tune in to the configured station (e.g. Horizon Pulse) and your songs should start playing.

Note about the Announcers (DJs): Even if you replace 100% of the music playlist, the DJs' commentary between tracks will remain in the game, as it lives in a separate, original audio bank of the game.