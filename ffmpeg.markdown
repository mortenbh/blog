# How to assemble a video from a sequence of images

Last modified: 24-04-2013 11:40:34

Working on computer graphics, I often find the need to make a movie out of a
sequence of images representing some animation. Fortunately, this is very easy
to do with `ffmpeg`.

The following command will make a 60 FPS movie out of all images named
`image_0000.png`, `image_0001.png` and so on:

	$ ffmpeg -r 60 -i image_%04d.png movie.mp4

Sometimes you do not have enough images in a sequence for 60 FPS (or even 30
FPS). It could be that you only have enough images to show a _single_ frame
per second. Nevertheless, you would still to make a movie. Unfortunately, most
players do not handle movies with really low frame rates very well, so if you
do the naive thing of just setting `-r 1` in the command above, playback will
likely be problematic. In this case it can be useful to specify a different
frame rate for the output movie:

	$ ffmpeg -r 1 -i image_%04d.png -r 30 movie.mp4
