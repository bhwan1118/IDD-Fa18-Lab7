# Video Doorbell, Lab 7

*A lab report by Benjamin Hwang*

### In This Report

1. Upload a video of your version of the camera lab to your lab Github repository
1. As usual, update your class Hub repository to add your [forked IDD-Fa18-Lab7](/FAR-Lab/IDD-Fa18-Lab7) repository.
1. Answer the questions in-line below on your README.md.

## Part A. HelloYou from the Raspberry Pi

**a. Link to a video of your HelloYou sketch running.**

[video demo](https://youtu.be/pxTsOFqgu90)

## Part B. Web Camera

**a. Compare `helloYou/server.js` and `IDD-Fa18-Lab7/pictureServer.js`. What elements had to be added or changed to enable the web camera? (Hint: It might be good to know that there is a UNIX command called `diff` that compares files.)**

One addition to the pictureServer.js is this block of code addign a function to take a picture using the webapp.

``` 
    socket.on('takePicture', function() {
    /// First, we create a name for the new picture.
    /// The .replace() function removes all special characters from the date.
    /// This way we can use it as the filename.
    var imageName = new Date().toString().replace(/[&\/\\#,+()$~%.'":*?<>{}\s-]/g, '');

    console.log('making a making a picture at'+ imageName); // Second, the name is logged to the console.

    //Third, the picture is  taken and saved to the `public/`` folder
    NodeWebcam.capture('public/'+imageName, opts, function( err, data ) {
    io.emit('newPicture',(imageName+'.jpg')); ///Lastly, the new name is send to the client web browser.
    /// The browser will take this new name and load the picture from the public folder.
  });
 ```

**b. Include a video of your working video doorbell**

[updated code](https://github.com/bhwan1118/IDD-Fa18-Lab7/blob/master/pictureServer_updated.js)
[video demo](https://youtu.be/1wY_D3BFyk8)

## Part C. Make it your own

**a. Find, install, and try out a node-based library and try to incorporate into your lab. Document your successes and failures (totally okay!) for your writeup. This will help others in class figure out cool new tools and capabilities.**

So I failed pretty hard at this. I don't think I fully understand how the packages work. But basically my attempt was trying to add a swirl effect to the webcam picture taken. I tried to accomplish this by using GraphicsMagick. I played around with the code and I think that the issue is that I don't exactly know how the gm() function outputs. My code is as follows:

```
parser.on('data', function(data){
  console.log('picturetaken', data);
  var imageName = new Date().toString().replace(/[&\/\\#,+()$~%.'":*?<>{}\s-]/g, '');

  console.log('making a making a picture at'+ imageName); // Second, the name is logged to the console.

  //code to edit the picture before it is shown
  var gm = require('gm')

  //Third, the picture is  taken and saved to the `public/`` folder
  NodeWebcam.capture('public/'+ imageName, opts, function( err, data ) {
  //io.emit('newPicture',(imageName+'.jpg')); ///Lastly, the new name is send to the client web browser.
  /// The browser will take this new name and load the picture from the public folder.
  var gm = require('gm')
  gm('public/'+ imageName + '.jpg').swirl(200)
  .write('public/' + imageName +'swirl'+'.jpg', function(err , data){
    if (!err) console.log('done');
    io.emit('swirl',(imageName +'swirl'+'.jpg'));
  });

});

```

Essentially I tried to edit the code so that I wrote a new file based on the image created by NodeWebcam.capture and used the gm function to handle that original image. After I wrote the new file I tried to use io.emit() to then display the image on the webapp. This did not work. And after several attempts at modifiying the code I was not able to make much progress otherwise. Sadly I have a midterm tomorrow so I will stop here. But I will say that it was a great learning experience given the time I could afford and I'm sure that I am somewhat close to something that would've worked.

**b. Upload a video of your working modified project**
