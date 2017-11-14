# phpGD
index.php

<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8" />
	<meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no"/>
	<title>二维码生成</title>
	<link type="text/css" rel="stylesheet" href="../css/frozen.css" />
</head>
<body >
<div class="wrapper">
	<img src="toutu.php?name=<?=$_GET['name'].'.jpg'?>" width="100%"/>
</div>
</body>
</html>


toutu.php
<?php	
	img();
	//用于对图片进行缩放
    function thumb($filename,$width=190,$height=190){
        //获取原图像$filename的宽度$width_orig和高度$height_orig
        list($width_orig,$height_orig) = getimagesize($filename);
        //根据参数$width和$height值，换算出等比例缩放的高度和宽度
        if ($width && ($width_orig<$height_orig)){
            $width = ($height/$height_orig)*$width_orig;
        }else{
            $height = ($width / $width_orig)*$height_orig;
        }
        //将原图缩放到这个新创建的图片资源中
        $image_p = imagecreatetruecolor($width, $height);
        //获取原图的图像资源
        $image = imagecreatefromjpeg($filename);
        //使用imagecopyresampled()函数进行缩放设置
        imagecopyresampled($image_p,$image,0,0,0,0,$width,$height,$width_orig,$height_orig);
        //将缩放后的图片$image_p保存，100(质量最佳，文件最大)
        imagejpeg($image_p,$filename);
        imagedestroy($image_p);
        imagedestroy($image);
    }
	//生成img图片	
	function img(){
		$name = $_GET['name'];	
		thumb('img/'.$name,190,190);
		header("content-type:image/jpeg");
		$name = $_GET['name'];
		$im = imagecreatetruecolor(600, 800);
		$bg = imagecreatefromjpeg('img/toutu.jpg');
		$t2 = 'img/'.$name;
		$im2 = imagecreatefromjpeg($t2);
		$black = imagecolorallocate($im, 0, 0, 0);
		imagecopymerge($bg, $im2, 378, 550, 0, 0,imagesx($im2), imagesy($im2), 100);
		imagecopy($im,$bg,0,0,0,0,600,800);
		imagejpeg($im); //生成jpg格式的图片输出给浏览器
		imagedestroy($im); //销毁图像资源，释放画布占用的内存空间
 }
?>
