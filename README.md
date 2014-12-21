osu!nplib
========

Exposes events for handling song change in osu!.

#Features
* Emits basic song change events from osu! client.
* Does not suppress other windows from getting events when used multiple times (i.e. passes SendMessage events to other applications which are using this library).

#Data Structs
```csharp
	public struct OSUMusic
	{
		public bool active;
		public OSUMusicStatus status;
		public string musicString;
		public DateTime createdAt;
		public int retries;
```

Field         | Description
-----------   | ------------
`active`      | `1` if osu! is active, `0` if osu! is shutting down.
`status`      | Possible values: `Inactive`, `Listening`, `Playing`, `Watching`.
`musicString` | Formatted as "Artist - Title [Difficulty]". _Note: Difficulty is displayed only when `status` is `Playing` or `Watching`_.
`createdAt`   | When `OSUMusic` object was created.
`retries`     | Retry counter for the song.

#Examples
```csharp
static void Main(string[] args)
{
	OSUMusicTracker tracker = new OSUMusicTracker();
	tracker.MusicUpdated += new OSUMusicTracker.MusicUpdate((OSUMusicTracker m, OSUMusic music) =>
	{
		string stringStatus = String.Empty;
		switch (music.status)
		{
			case OSUMusicStatus.Listening:
				stringStatus = "listening to";
				break;
			case OSUMusicStatus.Playing:
				stringStatus = "playing";
				break;
			case OSUMusicStatus.Watching:
				stringStatus = "watching";
				break;
		}
		Console.WriteLine(
		  (music.active ? "1" : "0") + "] " +
		  stringStatus + " " +
		  music.musicString
	  );
	});
	
	tracker.OnRetry += new OSUMusicTracker.BeatmapRetry(
		(OSUMusicTracker m, OSUMusic music) => {
		  Console.WriteLine("Retry");
		}
	);

	Application.Run();
}
```

#License
None yet.
