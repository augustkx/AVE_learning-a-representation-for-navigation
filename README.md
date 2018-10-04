# AVE_learning-a-representation-for-navigation
##Training VAEE
The training data is the seqence of image exracting from a house tour video in https://www.youtube.com/watch?v=jdNiWiXiJQ4\&t=200s

Here are the procedures for prearing training images:

1. cut the viseo into shorter pieces:
ffmpeg -ss 0:0:37 -i input.mp4 -t 0:3:20  -c copy out.mp4 

2. extract 1000 images from the cut video.
  ffmpeg -i out.mp4 -vf fps=1000/3.33*60 %d.png,

Note, that in the code you should change the address, where the training data is stored in your system. 

After the training data is ready. Run the training_model_vae_deconv _4d_latent.py,  you will get the visulaization of learned manifold and also a 'h5' file that stores the trained VAE model parameter information. This file will be used in the path planning stage.

##Ppath Planning
After the VAE model is learned, it can be used as a representation map for navigation.
Run the navigation_main.py file. In line 30 and 31, you can change the starting image and ending image. At the bottom of the file, there are some functions about path planning method, whose effects can be in four categories:

1.Initail path in Euclidean space, visualise path with generated images

2.Gradient descent method on the path sequence to update it to geodesic, and visualize the path with generated images 

3.Visualize the route with real frames according to generated images

4.Select a ground truth route by humans as the basis for evaluation

First, we try to produce a straigh line in the 4 D latent Euclidean space, connecting the staring point(in the latent space) and ending point(in the latent space). To do this, uncomment: navi.visualise_route('passway_straight.png',points,shrink,batch_size,latent_dim) in part 1.

Second, we can map this created path to the reality, by fitting the reconstructed image sequence (path) with the images in the training set. With the nearest neighbour metric, a corresponding path in reality is constructed. To do this, umcomment:  navi.visualise_route('passway_straight.png',points,shrink,batch_size,latent_dim) in part 1.
navi.route_reality('straight_line',original_img_size,shrink,points) in part 3.

Third, we try to update the path to geodesic by Gradient Descent method. To do this, umcomment: 
navi.visualise_route('passway_straight.png',points,shrink,batch_size,latent_dim) in the 1 part;
and points = navi.gd_points(points,num_p) in part 2;
and navi.visualise_route('passway_dg.png',points,shrink,batch_size,latent_dim) in part 2.


Forth, we can map this created path to the reality for the route visualization. To do this, umcomment: 
navi.visualise_route('passway_straight.png',points,shrink,batch_size,latent_dim) in part 1;
and points= navi.gd_points(points,num_p) in part 2;
and navi.visualise_route('passway_dg.png',points,shrink,batch_size,latent_dim) in part 2;
and navi.route_reality('Gredient_descent',original_img_size,shrink,points) in part 3.

Fifth, the path produced through the learned latent space manifold is reasonable and short, but not continously enough for robot navigate with computer vision techniques.To evaluate the resulting route, a ground truth path and route is generated by human selection.
To do this, uncomment: all the commands in part 4.





# Reference
https://github.com/keras-team/keras/blob/master/examples/variational_autoencoder_deconv.py




