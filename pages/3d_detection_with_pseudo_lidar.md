---
title: 3d detection with pseudo lidar
---

- [[3D Object Detection]] [[monocular]]
- 评分
- Monocular 3D Object Detection with Pseudo-LiDAR Point Cloud #toread
    - Zotero Metadata
        - * Item Type: [[Conference paper]]
        - * Authors: [[Xinshuo Weng]], [[Kris Kitani]]
        - * Proceedings Title: [[2019 IEEE/CVF International Conference on Computer Vision Workshop (ICCVW)]]
        - * Date: [[10/2019]]
        - [https://ieeexplore.ieee.org/document/9022330/](https://ieeexplore.ieee.org/document/9022330/)
        - * DOI: [10.1109/ICCVW.2019.00114](https://doi.org/10.1109/ICCVW.2019.00114)
        - * Cite key: wengMonocular3DObject2019
        - * Topics: [[monocular]]
        - #[[pseudo lidar point cloud]], #zotero #literature-notes #reference
        - PDF Attachments
    - [Weng and Kitani - 2019 - Monocular 3D Object Detection with Pseudo-LiDAR Po.pdf](zotero://open-pdf/library/items/9FGQZCRP)
        - [[abstract]]:
          Monocular 3D scene understanding tasks, such as object size estimation, heading angle estimation and 3D localization, is challenging. Successful modern day methods for 3D scene understanding require the use of a 3D sensor. On the other hand, single image based methods have signiﬁcantly worse performance. In this work, we aim at bridging the performance gap between 3D sensing and 2D sensing for 3D object detection by enhancing LiDAR-based algorithms to work with single image input. Speciﬁcally, we perform monocular depth estimation and lift the input image to a point cloud representation, which we call pseudoLiDAR point cloud. Then we can train a LiDAR-based 3D detection network with our pseudo-LiDAR end-to-end. Following the pipeline of two-stage 3D detection algorithms, we detect 2D object proposals in the input image and extract a point cloud frustum from the pseudo-LiDAR for each proposal. Then an oriented 3D bounding box is detected for each frustum. To handle the large amount of noise in the pseudo-LiDAR, we propose two innovations: (1) use a 2D-3D bounding box consistency constraint, adjusting the predicted 3D bounding box to have a high overlap with its corresponding 2D proposal after projecting onto the image; (2) use the instance mask instead of the bounding box as the representation of 2D proposals, in order to reduce the number of points not belonging to the object in the point cloud frustum. Through our evaluation on the KITTI benchmark, we achieve the top-ranked performance on both bird’s eye view and 3D object detection among all monocular methods, effectively quadrupling the performance over previous state-of-the-art. Our code is available at https: //github.com/xinshuoweng/Mono3D_PLiDAR.
        - zotero items: [Local library](zotero://select/items/1_V8QSUWPW)
    - Zotero Metadata
        - * Item Type: [[Conference paper]]
        - * Authors: [[Xinshuo Weng]], [[Kris Kitani]]
        - * Proceedings Title: [[2019 IEEE/CVF International Conference on Computer Vision Workshop (ICCVW)]]
        - * Date: [[10/2019]]
        - [https://ieeexplore.ieee.org/document/9022330/](https://ieeexplore.ieee.org/document/9022330/)
        - * DOI: [10.1109/ICCVW.2019.00114](https://doi.org/10.1109/ICCVW.2019.00114)
        - * Cite key: wengMonocular3DObject2019
        - * Topics: [[monocular]]
        -
        - [[pseudo lidar point cloud]] #zotero #literature-notes #reference
        - PDF Attachments
    - [Weng and Kitani - 2019 - Monocular 3D Object Detection with Pseudo-LiDAR Po.pdf](zotero://open-pdf/library/items/9FGQZCRP)
        - [[abstract]]:
          Monocular 3D scene understanding tasks, such as object size estimation, heading angle estimation and 3D localization, is challenging. Successful modern day methods for 3D scene understanding require the use of a 3D sensor. On the other hand, single image based methods have signiﬁcantly worse performance. In this work, we aim at bridging the performance gap between 3D sensing and 2D sensing for 3D object detection by enhancing LiDAR-based algorithms to work with single image input. Speciﬁcally, we perform monocular depth estimation and lift the input image to a point cloud representation, which we call pseudoLiDAR point cloud. Then we can train a LiDAR-based 3D detection network with our pseudo-LiDAR end-to-end. Following the pipeline of two-stage 3D detection algorithms, we detect 2D object proposals in the input image and extract a point cloud frustum from the pseudo-LiDAR for each proposal. Then an oriented 3D bounding box is detected for each frustum. To handle the large amount of noise in the pseudo-LiDAR, we propose two innovations: (1) use a 2D-3D bounding box consistency constraint, adjusting the predicted 3D bounding box to have a high overlap with its corresponding 2D proposal after projecting onto the image; (2) use the instance mask instead of the bounding box as the representation of 2D proposals, in order to reduce the number of points not belonging to the object in the point cloud frustum. Through our evaluation on the KITTI benchmark, we achieve the top-ranked performance on both bird’s eye view and 3D object detection among all monocular methods, effectively quadrupling the performance over previous state-of-the-art. Our code is available at https: //github.com/xinshuoweng/Mono3D_PLiDAR.
        - zotero items: [Local library](zotero://select/items/1_V8QSUWPW)