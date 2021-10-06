# COLMAP parameters


## Common parameters
Parameters shared by all the commands.
<details>
<summary>Show the list of parameters</summary>

**random_seed** (default: 0)\
Integer to use as the random seed to initialize the random number generator.

**log_to_stderr** (default: 0)

**log_level** (default: 0)\
Possible values: 0 (none), 1 (fatal), 2 (error), 3 (warn), 4 (info)\
Controls the verbosity of the messages generated by the [FLANN](https://github.com/flann-lib/flann) library functions.

**database_path**\
Path to database in which to store the extracted data.

</details>


## Feature extractor settings
**colmap feature_extractor**: Perform feature extraction or import features for a set of images.\
Read more: [Feature Detection and Extraction](https://colmap.github.io/tutorial.html#feature-detection-and-extraction)
<details>
<summary>Show the list of parameters</summary>

**image_path**\
Root path to folder which contains the images.

**image_list_path**\
Optional list of images to read. The list must contain the relative path of the images with respect to the image_path.

**ImageReader.mask_path**\
Optional root path to folder which contains image masks. For a given image, the corresponding mask must have the same sub-path below this root as the image has below image_path. The filename must be equal, aside from the added extension `.png`. For example, for an image `image_path/abc/012.jpg`, the mask would be `mask_path/abc/012.jpg.png`. No features will be extracted in regions where the mask image is black (pixel intensity value 0 in grayscale).

**ImageReader.camera_model** (default: `SIMPLE_RADIAL`)\
Possible values: `SIMPLE_PINHOLE`, `PINHOLE`, `SIMPLE_RADIAL`, `RADIAL`, `OPENCV`, `OPENCV_FISHEYE`, `FULL_OPENCV`, `FOV`, `SIMPLE_RADIAL_FISHEYE`, `RADIAL_FISHEYE`, `THIN_PRISM_FISHEYE`\
Name of the camera model.
See: [Camera Models](https://colmap.github.io/cameras.html)

**ImageReader.single_camera** (default: 0)\
Whether to use the same camera for all images.

**ImageReader.single_camera_per_folder** (default: 0)\
Whether to use the same camera for all images in the same sub-folder.

**ImageReader.single_camera_per_image** (default: 0)\
Whether to use a different camera for each image.

**ImageReader.existing_camera_id** (default: -1)\
Whether to explicitly use an existing camera for all images. Note that in this case the specified camera model and parameters are ignored.

**ImageReader.camera_params**\
Manual specification of camera parameters. If empty, camera parameters will be extracted from EXIF, i.e. principal point and focal length.

**ImageReader.default_focal_length_factor** (default: 1.2)\
If camera parameters are not specified manually and the image does not have focal length EXIF information, the focal length is set to the value `default_focal_length_factor * max(width, height)`.

**ImageReader.camera_mask_path**\
Optional path to an image file specifying a mask for all images. No features will be extracted in regions where the mask is black (pixel intensity value 0 in grayscale).

**SiftExtraction.num_threads** (default: -1)\
Number of threads for feature extraction.

**SiftExtraction.use_gpu** (default: 1)\
Whether to use the GPU for feature extraction.

**SiftExtraction.gpu_index** (default: -1)\
Index of the GPU used for feature extraction. For multi-GPU extraction, you should separate multiple GPU indices by comma, e.g. "0,1,2,3".
See: [Multi-GPU support in feature extraction/matching](https://colmap.github.io/faq.html#multi-gpu-support-in-feature-extraction-matching)

**SiftExtraction.max_image_size** (default: 3200)\
Maximum image size, otherwise image will be down-scaled.

**SiftExtraction.max_num_features** (default: 8192)\
Maximum number of features to detect, keeping larger-scale features.

**SiftExtraction.first_octave** (default: -1)\
First octave in the pyramid, i.e. -1 upsamples the image by one level. By convention, the octave of index 0 starts with the image full resolution. Specifying an index greater than 0 starts the scale space at a lower resolution (e.g. 1 halves the resolution). Similarly, specifying a negative index starts the scale space at an higher resolution image, and can be useful to extract very small features (since this is obtained by interpolating the input image, it does not make much sense to go past -1).

**SiftExtraction.num_octaves** (default: 4)\
Number of octaves. Increasing the scale by an octave means doubling the size of the smoothing kernel, whose effect is roughly equivalent to halving the image resolution. By default, the scale space spans as many octaves as possible (i.e. roughly `log2(min(width, height))`), which has the effect of searching keypoints of all possible sizes.

**SiftExtraction.octave_resolution** (default: 3)\
Number of levels per octave. Each octave is sampled at this given number of intermediate scales. Increasing this number might in principle return more refined keypoints, but in practice can make their selection unstable due to noise.

**SiftExtraction.peak_threshold** (default: 0.0067)\
Peak threshold for detection. This is the minimum amount of contrast to accept a keypoint. Increase to eliminate more keypoints.

**SiftExtraction.edge_threshold** (default: 10)\
Edge threshold for detection. Decrease to eliminate more keypoints.

**SiftExtraction.estimate_affine_shape** (default: 0)\
Estimate affine shape of SIFT features in the form of oriented ellipses as opposed to original SIFT which estimates oriented disks.

**SiftExtraction.max_num_orientations** (default: 2)\
Maximum number of orientations per keypoint if not `SiftExtraction.estimate_affine_shape`.

**SiftExtraction.upright** (default: 0)\
Fix the orientation to 0 for upright features.

**SiftExtraction.domain_size_pooling** (default: 0)\
Enable the more discriminative DSP-SIFT features instead of plain SIFT. Domain-size pooling computes an average SIFT descriptor across multiple scales around the detected scale. DSP-SIFT outperforms standard SIFT in most cases.\
This was proposed in [Domain-Size Pooling in Local Descriptors: DSP-SIFT](https://arxiv.org/abs/1412.8556), J. Dong and S. Soatto, CVPR 2015.
This has been shown to outperform other SIFT variants and learned descriptors in [Comparative Evaluation of Hand-Crafted and Learned Local Features](https://demuc.de/papers/schoenberger2017comparative.pdf), Schönberger, Hardmeier, Sattler, Pollefeys, CVPR 2016.

**SiftExtraction.dsp_min_scale** (default: 0.1667)\
**SiftExtraction.dsp_max_scale** (default: 3)\
**SiftExtraction.dsp_num_scales** (default: 10)\
Domain-size pooling parameters.
See: `SiftExtraction.domain_size_pooling`

</details>


## Exhaustive matcher settings
**colmap exhaustive_matcher**: Performs feature matching after performing feature extraction.\
Read more: [Feature Matching and Geometric Verification](https://colmap.github.io/tutorial.html#feature-matching-and-geometric-verification)
<details>
<summary>Show the list of parameters</summary>

**SiftMatching.num_threads** (default: -1)\
Number of threads for feature matching and geometric verification.

**SiftMatching.use_gpu** (default: 1)\
Whether to use the GPU for feature matching.

**SiftMatching.gpu_index** (default: -1)\
Index of the GPU used for feature matching. For multi-GPU matching, you should separate multiple GPU indices by comma, e.g. "0,1,2,3".
See: [Multi-GPU support in feature extraction/matching](https://colmap.github.io/faq.html#multi-gpu-support-in-feature-extraction-matching)

**SiftMatching.max_ratio** (default: 0.8)\
Maximum distance ratio between first and second best match.

**SiftMatching.max_distance** (default: 0.7)\
Maximum distance to best match.

**SiftMatching.cross_check** (default: 1)\
Whether to enable cross checking in matching.

**SiftMatching.max_error** (default: 4)\
Maximum epipolar error in pixels for geometric verification.

**SiftMatching.max_num_matches** (default: 32768)\
Maximum number of matches.

**SiftMatching.confidence** (default: 0.999)\
Confidence threshold for geometric verification.

**SiftMatching.max_num_trials** (default: 10000)\
Maximum number of RANSAC iterations. Note that this option overrules the `SiftMatching.min_inlier_ratio` option.

**SiftMatching.min_inlier_ratio** (default: 0.25)\
A priori assumed minimum inlier ratio, which determines the maximum number of iterations.

**SiftMatching.min_num_inliers** (default: 15)\
Minimum number of inliers for an image pair to be considered as geometrically verified.

**SiftMatching.multiple_models** (default: 0)\
Whether to attempt to estimate multiple geometric models per image pair.

**SiftMatching.guided_matching** (default: 0)\
Whether to perform guided matching, if geometric verification succeeds.

**ExhaustiveMatching.block_size** (default: 50)\
Block size, i.e. number of images to simultaneously load into memory.

</details>


## Mapper settings
**colmap mapper**: Sparse 3D reconstruction/mapping of the dataset using SfM after performing feature extraction and matching.
<details>
<summary>Show the list of parameters</summary>

**Mapper.min_num_matches** (default: 15)\
The minimum number of matches for inlier matches to be considered.

**Mapper.ignore_watermarks** (default: 0)\
Whether to ignore the inlier matches of watermark image pairs.

**Mapper.multiple_models** (default: 1)\
Whether to reconstruct multiple sub-models.

**Mapper.max_num_models** (default: 50)\
The number of sub-models to reconstruct.

**Mapper.max_model_overlap** (default: 20)\
The maximum number of overlapping images between sub-models. If the current sub-models shares more than this number of images with another model, then the reconstruction is stopped.

**Mapper.min_model_size** (default: 10)\
The minimum number of registered images of a sub-model, otherwise the sub-model is discarded.

**Mapper.init_image_id1** (default: -1)\
**Mapper.init_image_id2** (default: -1)\
The image identifiers used to initialize the reconstruction. Note that only one or both image identifiers can be specified. In the former case, the second image is automatically determined.

**Mapper.init_num_trials** (default: 200)\
The number of trials to initialize the reconstruction.

**Mapper.extract_colors** (default: 1)\
Whether to extract colors for reconstructed points.

**Mapper.num_threads** (default: -1)\
The number of threads to use during reconstruction.

**Mapper.min_focal_length_ratio** (default: 0.1)\
**Mapper.max_focal_length_ratio** (default: 10)\
**Mapper.max_extra_param** (default: 1)\
Thresholds for filtering images with degenerate intrinsics.

**Mapper.ba_refine_focal_length** (default: 1)\
**Mapper.ba_refine_principal_point** (default: 0)\
**Mapper.ba_refine_extra_params** (default: 1)\
Which intrinsic parameters to optimize during the reconstruction.

**Mapper.ba_min_num_residuals_for_multi_threading** (default: 50000)\
The minimum number of residuals per bundle adjustment problem to enable multi-threading solving of the problems.

**Mapper.ba_local_num_images** (default: 6)\
The number of images to optimize in local bundle adjustment.

**Mapper.ba_local_function_tolerance** (default: 0)\
Ceres solver function tolerance for local bundle adjustment

**Mapper.ba_local_max_num_iterations** (default: 25)\
The maximum number of local bundle adjustment iterations.

**Mapper.ba_global_use_pba** (default: 0)\
Whether to use PBA (Parralel Bundle Adjustment) in global bundle adjustment.
See: https://grail.cs.washington.edu/projects/mcba/, https://github.com/cbalint13/pba

**Mapper.ba_global_pba_gpu_index** (default: -1)\
The GPU index for PBA bundle adjustment.

**Mapper.ba_global_images_ratio** (default: 1.1)\
**Mapper.ba_global_points_ratio** (default: 1.1)\
**Mapper.ba_global_images_freq** (default: 500)\
**Mapper.ba_global_points_freq** (default: 250000)\
The growth rates after which to perform global bundle adjustment.

**Mapper.ba_global_function_tolerance** (default: 0)\
Ceres solver function tolerance for global bundle adjustment

**Mapper.ba_global_max_num_iterations** (default: 50)\
The maximum number of global bundle adjustment iterations.

**Mapper.ba_global_max_refinements** (default: 5)\
**Mapper.ba_global_max_refinement_change** (default: 0.0005)\
**Mapper.ba_local_max_refinements** (default: 2)\
**Mapper.ba_local_max_refinement_change** (default: 0.001)\
The thresholds for iterative bundle adjustment refinements.

**Mapper.snapshot_path**\
Path to a folder with reconstruction snapshots during incremental reconstruction. Snapshots will be saved according to the specified frequency of registered images.

**Mapper.snapshot_images_freq** (default: 0)\

**Mapper.fix_existing_images** (default: 0)\
If reconstruction is provided as input, fix the existing image poses.

#### Incremental Mapper parameters. Class that provides all functionality for the incremental reconstruction procedure.

**Mapper.init_min_num_inliers** (default: 100)\
Minimum number of inliers for initial image pair.

**Mapper.init_max_error** (default: 4)\
Maximum error in pixels for two-view geometry estimation for initial image pair.

**Mapper.init_max_forward_motion** (default: 0.95)\
Maximum forward motion for initial image pair.

**Mapper.init_min_tri_angle** (default: 16)\
Minimum triangulation angle for initial image pair.

**Mapper.init_max_reg_trials** (default: 2)\
Maximum number of trials to use an image for initialization.

**Mapper.abs_pose_max_error** (default: 12)\
Maximum reprojection error in absolute pose estimation.

**Mapper.abs_pose_min_num_inliers** (default: 30)\
Minimum number of inliers in absolute pose estimation.

**Mapper.abs_pose_min_inlier_ratio** (default: 0.25)\
Minimum inlier ratio in absolute pose estimation.

**Mapper.filter_max_reproj_error** (default: 4)\
Maximum reprojection error in pixels for observations.

**Mapper.filter_min_tri_angle** (default: 1.5)\
Minimum triangulation angle in degrees for stable 3D points.

**Mapper.max_reg_trials** (default: 3)\
Maximum number of trials to register an image.

**Mapper.local_ba_min_tri_angle** (default: 6)\
Minimum triangulation for images to be chosen in local bundle adjustment.

#### Incremental Triangulator. Class that triangulates points during the incremental reconstruction.

**Mapper.tri_max_transitivity** (default: 1)\
Maximum transitivity to search for correspondences.

**Mapper.tri_create_max_angle_error** (default: 2)\
Maximum angular error to create new triangulations.

**Mapper.tri_continue_max_angle_error** (default: 2)\
Maximum angular error to continue existing triangulations.

**Mapper.tri_merge_max_reproj_error** (default: 4)\
Maximum reprojection error in pixels to merge triangulations.

**Mapper.tri_complete_max_reproj_error** (default: 4)\
Maximum reprojection error to complete an existing triangulation.

**Mapper.tri_complete_max_transitivity** (default: 5)\
Maximum transitivity for track completion.

**Mapper.tri_re_max_angle_error** (default: 5)\
Maximum angular error to re-triangulate under-reconstructed image pairs.

**Mapper.tri_re_min_ratio** (default: 0.2)\
Minimum ratio of common triangulations between an image pair over the number of correspondences between that image pair to be considered as under-reconstructed.

**Mapper.tri_re_max_trials** (default: 1)\
Maximum number of trials to re-triangulate an image pair.

**Mapper.tri_min_angle** (default: 1.5)\
Minimum pairwise triangulation angle for a stable triangulation. If your images are taken from far distance with respect to the scene, you can try to reduce the minimum triangulation angle

**Mapper.tri_ignore_two_view_tracks** (default: 1)\
Whether to ignore two-view feature tracks in triangulation, resulting in fewer 3D points than possible. Triangulation of two-view tracks can in rare cases improve the stability of sparse image collections by providing additional constraints in bundle adjustment.

</details>


## Image undistorter settings
**colmap image_undistorter**: Undistorts images and/or exports them for MVS or to external dense reconstruction software, such as CMVS/PMVS.
<details>
<summary>Show the list of parameters</summary>

**blank_pixels** (default: 0)\
The amount of blank pixels in the undistorted image in the range [0,1].

**min_scale** (default: 0.2)\
Minimum and maximum scale change of camera used to satisfy the blank pixel constraint.

**max_scale** (default: 2)\
Minimum and maximum scale change of camera used to satisfy the blank pixel constraint.

**max_image_size** (default: -1)\
Maximum image size in terms of width or height of the undistorted camera.

**roi_min_x** (default: 0)\
**roi_min_y** (default: 0)\
**roi_max_x** (default: 1)\
**roi_max_y** (default: 1)\
The 4 factors in the range [0,1] that define the ROI (region of interest) in original image. The bounding box pixel coordinates are calculated as `(roi_min_x * Width, roi_min_y * Height)` and `(roi_max_x * Width, roi_max_y * Height)`.

</details>


## PatchMatch Stereo settings
**colmap patch_match_stereo**: Dense 3D reconstruction/mapping using MVS after running the image_undistorter to initialize the workspace.
<details>
<summary>Show the list of parameters</summary>

**PatchMatchStereo.max_image_size** (default: -1)\
Maximum image size in either dimension.

**PatchMatchStereo.gpu_index** (default: -1)\
Index of the GPU used for patch match. For multi-GPU usage, you should separate multiple GPU indices by comma, e.g. "0,1,2,3".
See: [Multi-GPU support in dense reconstruction](https://colmap.github.io/faq.html#multi-gpu-support-in-dense-reconstruction)

**PatchMatchStereo.depth_min** (default: -1)\
**PatchMatchStereo.depth_max** (default: -1)\
Depth range in which to randomly sample depth hypotheses.

**PatchMatchStereo.window_radius** (default: 5)\
Half window size to compute NCC photo-consistency cost.
Window radius is to measure the size of a patch concerning how many surrounding pixels should contribute to the reconstruction around a focusing pixel.

**PatchMatchStereo.window_step** (default: 1)\
Number of pixels to skip when computing NCC. For a value of 1, every pixel is used to compute the NCC. For larger values, only every n-th row and column is used and the computation speed thereby increases roughly by a factor of `window_step^2`. Note that not all combinations of window sizes and steps produce nice results, especially if the step is greather than 2.

**PatchMatchStereo.sigma_spatial** (default: -1)\
**PatchMatchStereo.sigma_color** (default: 0.2)\
Parameters for bilaterally weighted NCC.

**PatchMatchStereo.num_samples** (default: 15)\
Number of random samples to draw in Monte Carlo sampling.

**PatchMatchStereo.ncc_sigma** (default: 0.6)\
Spread of the NCC likelihood function.

**PatchMatchStereo.min_triangulation_angle** (default: 1)\
Minimum triangulation angle in degrees.

**PatchMatchStereo.incident_angle_sigma** (default: 0.9)\
Spread of the incident angle likelihood function.

**PatchMatchStereo.num_iterations** (default: 5)\
Number of coordinate descent iterations. Each iteration consists of four sweeps from left to right, top to bottom, and vice versa

**PatchMatchStereo.geom_consistency** (default: 1)\
Whether to add a regularized geometric consistency term to the cost function. If true, the `depth_maps` and `normal_maps` must not be null.

**PatchMatchStereo.geom_consistency_regularizer** (default: 0.3)\
The relative weight of the geometric consistency term w.r.t. to the photo-consistency term.

**PatchMatchStereo.geom_consistency_max_cost** (default: 3)\
Maximum geometric consistency cost in terms of the forward-backward reprojection error in pixels.

**PatchMatchStereo.filter** (default: 1)\
Whether to enable filtering.

**PatchMatchStereo.filter_min_ncc** (default: 0.1)\
Minimum NCC coefficient for pixel to be photo-consistent.

**PatchMatchStereo.filter_min_triangulation_angle** (default: 3)\
Minimum triangulation angle to be stable.

**PatchMatchStereo.filter_min_num_consistent** (default: 2)\
Minimum number of source images have to be consistent for pixel not to be filtered.

**PatchMatchStereo.filter_geom_consistency_max_cost** (default: 1)\
Maximum forward-backward reprojection error for pixel to be geometrically consistent.

**PatchMatchStereo.cache_size** (default: 32)\
Cache size in gigabytes for patch match, which keeps the bitmaps, depth maps, and normal maps of this number of images in memory. A higher value leads to less disk access and faster computation, while a lower value leads to reduced memory usage. Note that a single image can consume a lot of memory, if the consistency graph is dense.

**PatchMatchStereo.allow_missing_files** (default: 0)\
Whether to tolerate missing images/maps in the problem setup.

**PatchMatchStereo.write_consistency_graph** (default: 0)\
Whether to write the consistency graph.

</details>


## Stereo fusion settings
**colmap stereo_fusion**: Fusion of `patch_match_stereo` results into to a colored point cloud.
<details>
<summary>Show the list of parameters</summary>

**StereoFusion.mask_path**\
Path for PNG masks. Same format expected as `ImageReaderOptions`.

**StereoFusion.num_threads** (default: -1)\
The number of threads to use during fusion.

**StereoFusion.max_image_size** (default: -1)\
Maximum image size in either dimension.

**StereoFusion.min_num_pixels** (default: 5)\
Minimum number of fused pixels to produce a point.

**StereoFusion.max_num_pixels** (default: 10000)\
Maximum number of pixels to fuse into a single point.

**StereoFusion.max_traversal_depth** (default: 100)\
Maximum depth in consistency graph traversal.

**StereoFusion.max_reproj_error** (default: 2)\
Maximum relative difference between measured and projected pixel.

**StereoFusion.max_depth_error** (default: 0.01)\

**StereoFusion.max_normal_error** (default: 10)\
Maximum angular difference in degrees of normals of pixels to be fused.

**StereoFusion.check_num_images** (default: 50)\
Number of overlapping images to transitively check for fusing points.

**StereoFusion.cache_size** (default: 32)\
Cache size in gigabytes for fusion. The fusion keeps the bitmaps, depth maps, normal maps, and consistency graphs of this number of images in memory. A higher value leads to less disk access and faster fusion, while a lower value leads to reduced memory usage. Note that a single image can consume a lot of memory, if the consistency graph is dense.

**StereoFusion.use_cache** (default: 0)\
Flag indicating whether to use LRU cache or pre-load all data

</details>


## Delaunay mesher settings
**colmap delaunay_mesher**: Meshing of the reconstructed sparse or dense point cloud using a graph cut on the Delaunay triangulation and visibility voting.
<details>
<summary>Show the list of parameters</summary>

**DelaunayMeshing.max_proj_dist** (default: 20)\
Unify input points into one cell in the Delaunay triangulation that fall within a reprojected radius of the given pixels.

**DelaunayMeshing.max_depth_dist** (default: 0.05)\
Maximum relative depth difference between input point and a vertex of an existing cell in the Delaunay triangulation, otherwise a new vertex is created in the triangulation.

**DelaunayMeshing.distance_sigma_factor** (default: 1)\
The factor that is applied to the computed distance sigma, which is automatically computed as the 25th percentile of edge lengths. A higher value will increase the smoothness of the surface.

**DelaunayMeshing.quality_regularization** (default: 1)\
A higher quality regularization leads to a smoother surface.

**DelaunayMeshing.max_side_length_factor** (default: 25)\
**DelaunayMeshing.max_side_length_percentile** (default: 95)\
Filtering thresholds for outlier surface mesh faces. If the longest side of a mesh face (longest out of 3) exceeds the side lengths of all faces at a certain percentile by the given factor, then it is considered an outlier mesh face and discarded.

**DelaunayMeshing.num_threads** (default: -1)\
The number of threads to use for reconstruction. Default is all threads

</details>


## Poisson mesher settings
**colmap poisson_mesher**: Meshing of the fused point cloud using Poisson surface reconstruction.
<details>
<summary>Show the list of parameters</summary>

**PoissonMeshing.point_weight** (default: 1)\
This floating point value specifies the importance that interpolation of the point samples is given in the formulation of the screened Poisson equation. The results of the original (unscreened) Poisson Reconstruction can be obtained by setting this value to 0.

**PoissonMeshing.depth** (default: 13)\
This integer is the maximum depth of the tree that will be used for surface reconstruction. Running at depth d corresponds to solving on a voxel grid whose resolution is no larger than `2^d x 2^d x 2^d`. Note that since the reconstructor adapts the octree to the sampling density, the specified reconstruction depth is only an upper bound.

**PoissonMeshing.color** (default: 32)\
If specified, the reconstruction code assumes that the input is equipped with colors and will extrapolate the color values to the vertices of the reconstructed mesh. The floating point value specifies the relative importance of finer color estimates over lower ones.

**PoissonMeshing.trim** (default: 10)\
This floating point values specifies the value for mesh trimming. The subset of the mesh with signal value less than the trim value is discarded.

**PoissonMeshing.num_threads** (default: -1)\
The number of threads used for the Poisson reconstruction.

</details>
