# PDF-Coordinate-System-for-LibVGL
Simple PDF driver for LibVGL (Vector Graphic Library)
1. Coordinate system of LibVGL
The coordinate system of LibVGL is the same as Cartesian coordinate system in mathematics except the direction of y-axis. The y-axis extends downwards which is the same as the pixel coordinate system.

![Image](https://github.com/user-attachments/assets/0b4df8dd-f7cc-4755-91ef-a40125fa6a3a)

PDF coordinate system is the same as Cartesian coordinate system. The origin is left bottom. The x and y limits are the maximum value of paper size. If a paper is a letter, x extends to 8.5 inch in right direction, and y extends to 11 inch in upper direction. The goal of PDF driver is to convert the default coordinate of PDF to LibVGl coordinate. 

2. Understanding PDF Coordinate system.
<img width="2469" height="1244" alt="Image" src="https://github.com/user-attachments/assets/7b280cfd-c647-4a3d-a5df-a1476df07735" />

The default coordinate system of PDF is like the picture on the left side. There are two types of page direction: portrait and landscape. For portrait, reversing y direction is enough, but for landscape more steps are necessary to plot graphic elements. For landscape, the page must be rotated 90 degrees. This can be done in page dictionary (3 0 obj at the sample PDF file). When the page is rotated to 90 degrees, the axis geometry is like the picture on the right side. Then there is nothing you can do to draw graphic elements in this stage. You must use CTM to the whole graphic elements to place them on LibVGL coordinates system. There are three steps: translate, rotate, and reverse y direction. 
   
<img width="2839" height="2195" alt="Image" src="https://github.com/user-attachments/assets/a06c0bdd-28d1-4690-8011-b2b5dfc4480b" />

4. Results
```Python
import libvgl as vgl
fmm  = vgl.FrameManager()
frm = fmm.create(1,1,6,6, vgl.Data(0,1,0,1))
dev = vgl.DevicePDF("1.pdf", fmm.get_gbbox(),pdir='L',compression=False)
dev.set_device(frm)
dev.line(0,0,.8,.8,lcol=vgl.BLUE, lthk=0.01)
vgl.draw_axis(dev)
dev.close()
```

![Image](https://github.com/user-attachments/assets/d1174a82-d7be-432f-b227-0b53d82c1add)

![Image](https://github.com/user-attachments/assets/e0c7a8f5-ffe8-4d39-aab6-fad2d5dc4d37)
