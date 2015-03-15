# summary Some information about my SqueezeCenter module for Crestron control systems.

# Introduction 
Please enjoy and let me know if you have any questions or find any problems. The latest code is available in the Source tab, not Downloads.

## Important advice
  * Make sure you define the analog `NumberOfList` input (and other `NumberOf__` inputs), which is required to tell the module how many lines of text to output for a single page; if undefined, zero lines of text are generated.

  * Make sure you use only lowercase letters for the PlayerID MAC address.

  * Make sure you use Triode's Spotify plugin for LMS, not the official Spotify plugin. Triode's may be installed in the LMS settings web page by adding this repository: http://triodeplugins.googlecode.com/svn/trunk/repo.xml

## Revision History
|Version|Notes|
|-------|-----|
|0.7| Current working code. Added direct Spotify access and corrected some Spotify cover art. I also added an option (set as a parameter) to change what happens when you Select a track; the new option allow you to insert and play just the track, rather than replacing the playlist with the entire album from which the track comes.|
|0.6| Added current song bitrate and sample size.|
|0.5.4| Compatible with 3-series processors and with the CoverID method of cover art in SBS 7.6. Also adds a constant at the top to define the size of the arrays, allowing for lists of longer than ten positions if desired. Also has working Spotify cover art using the third-party plugin. |
|0.5.3| Moves the incoming data to a gather() within a while(1) loop in main. That eliminates the SocketReceive event, which should improve processor load. Also added ShowBriefly outputs (a serial for the text and a digital that pulses when a new message appears); a parameter (`suppressShowbrieflyPause`) controls whether pause messages are shown.  Currently available in the source tab. |
|0.5.2| Current testing build in repository; fixes list item covers for large collections; adds button emulation for SB remote; added option to always start playlists from the beginning (required for non-SBS playlists); starting to incorporate the CoverID for track covers, a change that is encouraged in SBS 7.6. |
|0.5.1| Adds mixer volume; user-defined size for current cover. |
|0.5| Adds support for extended characters and new option, `ASCII_only`. Adds custom home list, where there are parameters to set the number of items on the home list and pick what each one is. Adds the home list item "New Albums", which shows albums by the date they were added to the collection (most recent first). Adds CustomBrowse integration (a little rough due to the way that CB reports information). Adds the `ListPlayable` outputs to indicate whether each `List$` item is playable.||
|0.4.3|Corrects a few bugs: browsing internet music sources, `InPageNowplaying`. Adds `Playlist_Clear` input. Revision to the `Nowplaying` consolidated output. Adds trackstat rating capability. ||
|0.4.2|Corrects the bug above and adds a consolidated `CurrentSong` output, with options to set whether you want the consolidated output or separate output (or both).  Also has an option for whether you want time feedback. Time feedback has been substantially changed, so it now uses an internal timer to increment the time, with updates every 15 seconds from the server.||
|0.4.1|***NOTICE*** This version has a problem with album names that contain an escaped character near the end. It has been corrected in 0.4.2||
|0.4|BIG REVISION.  Includes multi-player control and coverflow mode, which switches to a different number of items to list and includes artists and cover URLs with the albums.  Also can list radios/apps and search in radios.  Search functionality changed to take a full string, so you now use the ASCII keypad symbol in SIMPL. Can use search functionality to perform functions in apps, like creating Pandora stations. Includes a combined output for the Nowplaying list, which shows "Trackname - Artist" as one output, with analogs setting the max length of each.  Similarly, there are analog inputs for the max width of the coverflow outputs.  I will try to (soon) update the signal descriptions below, which are now outdated. Contact me with any questions.||
|0.3.7|Fixed page up/down in home screen when list length is shorter than home screen||
|0.3.6|more bug fixes||
|0.3.5|Fixed the generation of titles for the lists of Pandora and Favorites pages; now generated and updated during the parsing sequence to reflect the actual data from the server. Also changed the mechanics of the input arrays for selecting, adding, inserting, etc. so it is more efficient code.||
|0.3.4|Added a direct input to play favorites with a string, e.g., "0.1" to play the second item in the first folder in favorites.  If the item does not exist, nothing happens. I use this to program keypad buttons (the first favorite is a folder called "keypads", which makes it really easy to reassign keypad playlists.  Also fixed a bug with favorites collections containing folders.||
|0.3.3|Fixed an issue with Favorites when it received empty results. Also changed the page up/down functions to avoid the blank pages that used to appear occasionally.||


# Module Inputs and Outputs
Here is a description of the various inputs and outputs:
## Connection
### Inputs
 
| Type | Name | Description |
|------|------|-------------|
|D|`TCPIP_Connect`||Causes the module to connect. It may be best to disconnect when not using as an active source to reduce traffic and load on the processor.|
|D|`TCPIP_ReconnectEnable`||Causes the module to attempt reconnection if the socket is closed remotely. I leave a 1 on this line.|

  ### Outputs
  
| Type | Name | Description |
|------|------|-------------|
|D|`TCPIP_Connected`|High if the module is connected to the Squeezebox Server|
|A|`TCPIP_Status`| analog value of connection status|

----

==Options==
  ===Inputs===
  
|| *Type* || *Name* || *Description* ||
|| P || `PlayerID$_Default` ||  The MAC address of the player to use on startup, e.g., "00:04:20:00:b7:01" (without the quotes). NOTE: use only lowercase letters. ||
|| P || `ServerIPAddr$` || String of the IP address of the server, e.g., "192.168.1.50" (without the quotes). ||
|| P || `ServerPort` ||  Integer value of the server CLI port. Default is 9090. ||
|| P || `volumeIncrement` ||  Controls the amount that volume changes with a push of the digital input. ||
|| P || `suppressShowbrieflyPause` || Controls whether showbriefly messages are shown for pause/unpause.  ||
|| P || `ListWidth` || Number of characters in the browse list items.  ||
|| P || `CurrentsongWidth` || Number of characters in current song fields.  ||
|| P || `NowplayingWidthArtist` || Number of characters in Nowplaying portion for artist (in the consolidated output).  ||
|| P || `NowplayingWidthTotal` ||  Number of characters in the total consolidated Nowplaying output. ||
|| P || `CoverflowWidthArtist` || Number of characters in the Artist line when in coverflow mode.  ||
|| P || `CoverflowWidthAlbum` || Number of characters in the Album line when in coverflow mode.  ||
|| P || `ListCoverSize` || Defines the size in pixels of the image for each list item, used if `ListCoverURL_Enable` is high. ||
|| P || `CoverflowSize` || Defines the size in pixels of the image for each album cover in coverflow mode (when `Coverflow` is high). ||
|| P || `CurrentsongCoverSize` || size in pixels of the current song cover.  ||
|| P || `NowplayingCoverSize` || Defines the size in pixels of the image for each Nowplaying item, used if `NowplayingCoverURL_Enable` is high. ||
|| P || `HomeListLength` || Defines the number of items on the home list. ||
|| P || `HomeItem[i]` || Parameters to select what each position in the home list is. There is a dropdown box to choose from the available options.  ||
|| D || `OnDisconnect_Pause` || Causes the player to pause when you set the `TCPIP_Connect` line low. Use to auto-pause when the source is not active. ||
|| D || `OnConnect_Play` || Causes the player to play when the connection is made to the server. Use to auto-play when the source is selected. ||
|| D || `OnConnect_DefaultPlayer` || Causes the module to switch to the player set by the `PlayerID$_Default` parameter. Use to automatically switch back to the default player if the module has been used with another player. ||
|| D || `DirectIn_DefaultPlayerOnly` || Causes the serial inputs that directly play `Favorites`/`Playlists`/`DynamicPlaylists` to always use the player defined by the `PlayerID$_Default` parameter. Use to allow keypad functionality even if the module is being used on a touchpanel for a different zone.||
|| D || `Playlist_Save_Enable` || Enables automatic saving of the playlist when playback is stopped or another playlist is started. The Automatic save feature detects certain changes made to the playlist (if through the Crestron module) and will not save in those instances. The purpose is to save the playback position so that if the playlist is resumed later, it will start where you left off. When enabled, the autosave features saves the playlist if there is an active playlist when entering the pause state or if a different playlist is resumed using the `PlaylistReume$` input. The active playlist is set when playing (using `ListSelect` or `ListPlay`) a playlist from the Browse list. The active playlist is cleared when playing a dynamic playlist, when playing an item (using `ListSelect` or `ListPlay`; other than a playlist) from the Browse list, adding or inserting an item from the Browse list, or removing an item from the Nowplaying list. ||
|| D || `Playlist_NeverResume` || Uses the "Play" command for playlists, rather than the "resume" command; required for playlists that are not created in SBS and sometimes seems more reliable. Disadvantage is that you cannot start a playlist from where you left off last time. ||
|| D || `NowplayingSeparateOuts_Enable` || Enable the `NowplayingTitle`/`Artist`/`Album` outputs. ||
|| D || `NowplayingConsolidatedOut_Enable` || Enable the Nowplaying outputs, which consist of "Title - Artist", trimmed to the maximum length defined by the analog input `NowplayingWidthTotal`, where the Artist trimmed to the analog input `NowplayingWidthArtist` if necessary.||
|| D || `BrowseCS_AlwaysSearch` || Causes the `BrowseCurrentGenre`/`Artist`/`Album` inputs to perform a search for the text of the `CurrentsongGenre`/`Artist`/`Album` output rather than look for the exact Genre/Artist/Album of the current song. The module will use this search behavior when playing an internet music source; the option input here causes it to do so always.||
|| D || `CurrentSongTime_Enable` || If you want to show the song time (ie 4:19 of 4:20) then this should be set to 1. Setting it to 0 will lessen the load on the processor because the module will not have to count the time internally and periodically poll the SB server (every 15 seconds currently). ||
|| D || `CurrentSongSeparateOut_Enable` || Enable the `CurrentsongTitle`/`Album`/`Artist`/`Genre` outputs. ||
|| D || `CurrentSongConsolidatedOut_Enable` || Enable a combined output of the `Currentsong` fields, which includes linefeeds and excludes missing information rather than the "<___ n/a>" output by the `Currentsong` separate outs. ||
|| D || `ListCoverURL_Enable` || Enables the output of a URL for each list item when possible. Not a complete feature yet.||
|| D || `FavoritesAdd_toEnd` || Adds favorites to the end of the favorites list rather than the beginning.||
|| D || `ASCII_only` || Set to 1 if you want the old, ASCII-only mode, where special characters are replaced with question marks. Otherwise, extended characters will be parsed according to the UTF-8 character set, as far as supported by Crestron.||
|| A || `TrackstatIncrement` || Defines the amount that a Rate_up or Rate_dn command changes the Trackstat rating of the current song. ||
|| A || `NowplayingWidthArtist` ||  Defines the maximum width of the Artist in the `Nowplaying` consolidated output. Artist is not trimmed if not necessary to meet the width defined by `NowplayingWidthTotal`. ||
|| A || `NowplayingWidthTotal` ||  Defines the maximum width of the `Nowplaying` consolidated output.  ||
|| A || `CoverflowWidthArtist` || Defines the maximum width of the `ListCoverflowArtist` output.  ||
|| A || `CoverflowWidthAlbum` ||  Defines the maximum width of the List$ output when in `Coverflow` mode. ||
|| A || `NumberOfList` || Defines the number of `List$` items to output.  ||
|| A || `NumberOfCoverflow` ||  Defines the number of `List$` items to output when in `Coverflow` mode. ||
|| A || `NumberOfNowplaying` ||  Defines the number of `Nowplaying` items to output. ||
|| A || `NumberOfPlayers` || Defines the number of `Players` items to output.  ||
|| A || `NowplayingShowPrev` || Controls how many lines before the previous song the `Nowplaying` list will show at its default position.  Allows you to see the last (1, 2,... n) songs played. ||

  ===Outputs===

|| *Type* || *Name* || *Description* ||
|| - || - || none currently. ||

----

==Transport Controls==
  ===Inputs===
|| *Type* || *Name* || *Description* ||
|| D || `Play` || Start playing. ||
|| D || `Pause_On` || Engage pause. ||
|| D || `Pause_Off` || Disengage pause. Generally equivalent to `Play`.||
|| D || `Pause_Tog` || Toggles the pause state. ||
|| D || `Stop` || Stops playback. ||
|| D || `Next` || Advances to the next track in the current playlist. ||
|| D || `Prev` || Goes to the previous track in the current playlist. ||
|| D || `Pwr_On` || Switches the player's power on. ||
|| D || `Pwr_Off` || Switches the player's power off. ||
|| D || `VolumeUp` || Increases player volume. ||
|| D || `VolumeDn` || Decreases player volume. ||
|| A || `VolumeBar` || Directly sets player volume from input ranging 0-65535.||
|| D || `Button__` || Emulates a number of buttons on the SB remote. ||
|| D || `Repeat_Off` || Sets repeat off. ||
|| D || `Repeat_Track` || Sets repeat to loop the current track. ||
|| D || `Repeat_All` || Sets repeat to loop the current playlist. ||
|| D || `Shuffle_Off` || Sets shuffle off. ||
|| D || `Shuffle_Track` || Shuffles the current playlist by track. ||
|| D || `Shuffle_Album` || Shuffles the current playlist by album. ||
|| D || `Playlist_Clear` || Clears the current Nowplaying list. ||
|| D || `Currentsong_AddFavorite` || Adds the currently playing song to the Favorites list, either at the top or bottom as determined by the option input `FavoriteAdd_toEnd`.||
|| D || `Rate_Up` || When rateable content is playing, sends a thumbs-up for the current track. If the track is local, increments the Trackstat rating by the amount defined by analog input TrackstatIncrement. ||
|| D || `Rate_Dn` || When rateable content is playing, sends a thumbs-down for the current track.  If the track is local, increments the Trackstat rating by the amount defined by analog input TrackstatIncrement. ||
|| A || `CurrentsongTrackstatSet` || Sets the Trackstat rating of the current song, if that song is a local file. Valid range is 0-100; if that range is exceeded then the module limits the rating to that range.  ||

  ===Outputs===
|| *Type* || *Name* || *Description* ||
|| || *Player state feedback* || ||
|| S || `CurrentPlayerName` || Name of current player. ||
|| S || `CurrentPlayerID` || ID (MAC address) of current player. ||
|| D || `CurrentPlayerPower` || Power state of current player (1 if on). ||
|| D || `CurrentPlayerConnected` || Connection state of current player (1 if connected).||
|| S || `CurrentPlayerVolume` || Text (0-100) of current player volume. ||
|| A || `CurrentPlayerVolumeBar` || Analog (0-65535) of current player volume. ||
|| D || `CurrentPlayerMaster` || 1 if current player is the master of a sync group. ||
|| D || `CurrentPlayerSlave` || 1 if current player is a slave in a sync group. ||
|| S || `CurrentPlayerMasterName` || Name of the player that is the master of the current sync group. ||
|| D || `mode_play_fb` || High if the player is playing. ||
|| D || `mode_pause_fb` || High if the player is paused. ||
|| D || `mode_stop_fb` || High if the player is stopped. ||
|| D || `repeat_off_fb` || High if the player is set to not repeat. ||
|| D || `repeat_track_fb` || High if the player is set to loop the current track. ||
|| D || `repeat_all_fb` || High if the player is set to loop the current playlist. ||
|| D || `shuffle_off_fb` || High if the player is set to not shuffle. ||
|| D || `shuffle_track_fb` || High if the player is set to shuffle the current playlist by track. ||
|| D || `shuffle_album_fb` || High if the player is set to shuffle the current playlist by album. ||
|| D || `showbriefly` || Pulses high for 3 seconds when a new showbriefly message arrives. ||
|| S || `showbriefly$` || the text of the latest showbriefly message. ||
|| || *Current song feedback* || ||
|| D || `CurrentsongRemote_fb` || High if current song is from the internet. ||
|| D || `CurrentsongIsRadioRateable` || High if the current track is rateable. Useful to control appearance of a subpage with the rating controls. ||
|| S || `CurrentsongTitle` || The track title of the current song. ||
|| S || `CurrentsongTitleFormatted` || A string for the current song formatted according to the setup on the web interface, I believe it is what the player scrolls on the screen. ||
|| S || `CurrentsongAlbum` || The album of the current song. ||
|| S || `CurrentsongArtist` || The artist of the current song. ||
|| S || `CurrentsongGenre` || The genre of the current song. ||
|| S || `CurrentsongCoverURL` || A URL to access the cover art for the current song. ||
|| S || `CurrentsongTime` || The current playback position of the current song. ||
|| S || `CurrentsongDuration` || The total duration of the current song. ||
|| S || `CurrentsongBitrate` || The bitrate of the current song. ||
|| S || `CurrentsongConsolidatedText` || Automatically formats the current song text metadata (title, album, artist, genre) on a single string so that missing metadata info is not displayed, and it separates the lines automatically with a carriage return.||
|| A || `CurrentsongTrackstat` || Value of the current song trackstat rating. Ranges from 0-100.||
|| S || `CurrentsongTrackstat$` || Serial output with the trackstat rating, rangin from "0" to "100", if the song is a local file. If not, it is a " " to clear the touchpanel text. ||
|| A || `CurrentsongType` || Indicates the type of the current song, as follows: 0 = local file; 1 = Pandora; 2 = Slacker; 3 = LastFM; 4 = Rhapsody; 5 = LiveMusicArchive; 6 = Sirius; 7 = Live365; 8 = Mediafly; 9 = Internet Radio/Podcast; 10 = Sounds&Effects. ||

----
==Browse Controls==
  ===Inputs==
|| *Type* || *Name* || *Description* ||
|| || *Browse List Items* || ||
|| D || `List_PgUp` || Moves the Browse list up a page. ||
|| D || `List_PgDn` || Moves the Browse list down a page. ||
|| D || `List_Back` || Moves the Browse list back in history. Hits the home page after a maximum of five steps. ||
|| A || `InPageList` || Scrolls the Browse list to a position.  Used to accept the analog feedback from a scrollbar on a GUI. ||
|| D || `Jump_Home` || Jumps the Browse list to the home page. ||
|| D || `Jump_Genres` || Jumps the Browse list to display all genres. ||
|| D || `Jump_Artists` || Jumps the Browse list to display all artists. ||
|| D || `Jump_Albums` || Jumps the Browse list to display all albums. ||
|| D || `Jump_Tracks` || Jumps the Browse list to display all tracks. ||
|| D || `Jump_Playlists` || Jumps the Browse list to display all playlists. ||
|| D || `Jump_Dynamic` || Jumps the Browse list to display all dynamic playlists. ||
|| D || `Jump_Pandora` || Jumps the Browse list to display the Pandora top page. ||
|| D || `Jump_Favorites` || Jumps the Browse list to display all Favorites. ||
|| D || `Jump_Radios` || Jumps the Browse list to display the internet radios page. ||
|| D || `BrowseCurrentSongGenre` || Jumps the Browse list to display all artists within the genre of the currently playing song. ||
|| D || `BrowseCurrentSongArtist` || Jumps the Browse list to display all albums by the artist of the currently playing song. ||
|| D || `BrowseCurrentSongAlbum` || Jumps the Browse list to display all tracks in the album of the currently playing song. ||
|| D || `Coverflow` ||  Engages coverflow mode, which starts the ListCoverURL outputs and switches to browsing albums. The scope of browsing depends on where you are when you engage coverflow. If you have restricted browsing to a genre, shows all albums for the genre. If you have restricted browsing to an artist, shows all albums for that artist. If you are in search results, runs the current search query on all album names. ||
|| S || `PlaylistPlay$` || Accepts the name of a playlist to begin playing immediately. ||
|| S || `PlaylistResume$` || Accepts the name of a playlist to begin playing immediately, beginning with the position playing when the playlist was last saved using Squeezebox Server. Note: this will not play playlists saved with something other than Squeezebox Server. ||
|| S || `FavoritePlay$` || Accepts the hierarchal position of a favorite you want to play. For example, submitting "0.1" to this input plays the second item in the first folder. ||
|| S || `DynamicResume$` || Begins playing a Dynamic playslist. If it was already playing, keep playing it. The string must be, e.g., "sqlplaylist_playlistname". ||
|| D || `ListSelect` || An array to select a selected item of the Browse list. The action taken depends on the item. If it is not a track, the Browse list will advance into the item selected. It the item is a track, the track's album replaces the current playlist and is played beginning with the selected track. ||
|| D || `ListPlay` || An array to play a selected item of the Browse list.  The selected item is inserted into the playlist following the currently playing song and playback is advanced to the newly inserted item. ||
|| D || `ListAdd` || An array to add a selected item of the Browse list to the end of the current playlist. ||
|| D || `ListInsert` || An array to insert a selected item of the Browse list as the next item in the current playlist. ||
|| || *Nowplaying list items* || ||
|| D || `Nowplaying_Refresh` || Refreshes the Nowplaying list to its default position, controlled byt he `NowplayingShowPrev` input (see Options). ||
|| D || `Nowplaying_PgUp` || Moves the Nowplaying list up a page. ||
|| D || `Nowplaying_PgDn` || Moves the Nowplaying list down a page. ||
|| A || `InPageNowplaying` || Scrolls the Nowplaying list to a position.  Used to accept the analog feedback from a scrollbar on a GUI. ||
|| D || `NowplayingPlay` || An array to play a selected item in the Nowplaying list. Does not change the playlist items, just changes which song is playing. ||
|| D || `NowplayingRemove` || An array to remove a selected item from the current playlist. ||
|| D || `NowplayingMoveUp` || An array to move a selected item up in the current playlist. ||
|| D || `NowPlayingMoveDown` || An array to move a selected item down in the current playlist. ||

  ===Outputs===
|| *Type* || *Name* || *Description* || 
|| || *Browse List items* || ||
|| S || `ListName$` || The title of the current list. Shows the name of the album if you are browsing the titles in the album, or the name of the genre if you are browsing the artists in the genre. Also provides search string feedback if you are searching. ||
|| S || `List$` || An array of items comprising the Browse list. ||
|| D || `ListPlayable` || An array indicating whether each `List$` item is playable (with the corresponding `ListPlay` input). Most useful for internet sources. ||
|| A || `OutListCount` || The number of items returned in the current list. ||
|| A || `OutListBar` || Feedback for a scrollbar of the Browse list position ||
|| S || `ListName$` || The title of the current list. Shows the name of the album if you are browsing the titles in the album, or the name of the genre if you are browsing the artists in the genre. Also provides search string feedback if you are searching. ||
|| S || `ListCoverURL` || The URL for the album cover of the list item. Only active when Coverflow input is high. ||
|| S || `ListCoverflowArtist` || The Artist of the list item (which is an album). Only active when Coverflow input is high. ||
|| || *Nowplaying List items* || ||
|| S || `NowplayingTitle$` || The Nowplaying list array of track names. ||
|| S || `NowplayingArtist$` || The Nowplaying list array of artist names. ||
|| S || `NowplayingAlbum$` || The Nowplaying list array of album names. ||
|| D || `NowplayingCurrentSong` || An array indicating if a particular position of the Nowplaying list is the current song ||
|| A || `OutNowplayingCount` || The number of items in the Noyplaying list. ||
|| A || `OutNowplayingBar` || Feedback for a scrollbar of the Nowplaying list position. ||
|| D || `NowplayingPageFlip` || Pulses in certain circumstances to indicate a flip to the Nowplaying page.  Useful for single-list GUIs, so when a playlist is selected, it plays the playlist and flips you to nowplaying. ||

----
==Search Controls==
  ===Inputs===
|| *Type* || *Name* || *Description* ||
|| S || `search_in$` || Strings here are used as the search string. Use an "ASCII Keypad" symbol in SIMPL to put together strings from button pushes. ||
|| D || `Search_Submit` || Submits the last search_in$ as a search. Not necessary for local searches, which occur every time search_in$ changes, but necessary when the `Search_RequiresSubmit` output is high (for internet searches). ||
<wiki:comment>
|| D || `search_clear` || Clears the search string. Best used when pulling up the keyboard on a panel, to start a new search. ||
|| D || `search_backspace` || Removes the last character of the search string and updates the display. ||
|| S || `search_in$` || String characters put here are appended to the end of the search string, and the Bowse list is updated with the results. That means it is find as you type. The the current list is on the home screen, search looks anywhere in the database. If the current list is in a sub-field, e.g., artist, then seach looks only in that field. ||
</wiki:comment>

  ===Outputs===
|| *Type* || *Name* || *Description* ||
|| D || `Search_KeyboardPopup` ||  Pulses when a search item in the list is selected, which prepares the module to accept a string an requires the input `Search_Submit` to run the search. ||
|| D || `Search_RequiresSubmit` ||  Indicates when a search requires the input `Search_Submit` to run the search. Used for searches of remote data, rather than the local database, which is find-as-you-type. ||
<wiki:comment>
|| S || `search_in_fb` || Shows the current search string. || # commented out because removed but may add again
</wiki:comment>

----

==Player Controls==
  ===Inputs===
  
|| *Type* || *Name* || *Description* ||
|| D || `Select_DefaultPlayer` || Selects the player defined by the parameter `PlayerID$_default`.  ||
|| D || `Players_Refresh` || Retrieves a listing of players from the server.  ||
|| D || `Players_PgUp` || Scroll the Players list.  ||
|| D || `Players_PgDn` ||  Scroll the Players list. ||
|| A || `InPagePlayers` ||  Scroll the Players list. ||
|| D || `PlayersSelect` ||  Selects a player from the Players list. ||
|| D || `PlayersSyncTog` ||  Toggles whether the selected player is Synced to the player currently being controlled.  If the currently controlled player is synced, un-syncs the player. ||

  ===Outputs===

|| *Type* || *Name* || *Description* ||
|| D || `CurrentPlayerPower` || High if the current player is powered on.  ||
|| D || `CurrentPlayerConnected` || High if the current player is connected to the server.  ||
|| D || `CurrentPlayerMaster` || High if the current player is a master in a sync group.  ||
|| D || `CurrentPlayerSlave` ||  High if the current player is a slave in a sync group. ||
|| D || `PlayersSynced` || Indicates which players are synced to the player currently being controlled.  ||
|| S || `CurrentPlayerName` || Indicates the name of the current player. ||
|| S || `CurrentPlayerID` || Indicates the ID of the current player. ||
|| S || `CurrentPlayerMasterName` || Indicates the name of the player that is the Master of the current sync group. ||
|| S || `Players$` ||  The names of the players connected to the server. ||
|| S || `PlayersID` ||  The ID of the players connected to the server. ||

The file radio.png is a replacement for the file of the same name distributed with SBS. It is made to match the style of the other top-level icons for use on the home list of this Crestron module. To use it, replace the existing file with this version (must be done after every upgrade of SBS).

For example, on Ubuntu, I use the following command (in which you must substitute the actual file location for <original_file_location>):
sudo cp <original_file_location> /usr/share/squeezeboxserver/HTML/Default/html/images/radio.png
