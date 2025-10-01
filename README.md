# PDF-Coordinate-System-for-LibVGL
Simple PDF driver for LibVGL (Vector Graphic Library)
1. Coordinate system of LibVGL
The coordinate system of LibVGL is the same as Cartesian coordinate system in mathematics except the direction of y-axis. The y-axis extends downwards which is the same as the pixel coordinate system.

![Image](https://github.com/user-attachments/assets/0b4df8dd-f7cc-4755-91ef-a40125fa6a3a)

PDF coordinate system is the same as Cartesian coordinate system. The origin is left bottom. The x and y limits are the maximum value of paper size. If a paper is a letter, x extends to 8.5 inch in right direction, and y extends to 11 inch in upper direction. The goal of PDF driver is to convert the default coordinate of PDF to LibVGl coordinate. 

2. Understanding PDF Coordinate system.
![Image](https://github.com/user-attachments/assets/156e03ad-3bc3-41cc-a767-0fa5d0cc38dd)

The default coordinate system of PDF is like the picture on the left side. There are two types of page direction: portrait and landscape. For portrait, reversing y direction is enough, but for landscape more steps are necessary to plot graphic elements. For landscape, the page must be rotated 90 degrees. This can be done in page dictionary (3 0 obj at the sample PDF file). When the page is rotated to 90 degrees, the axis geometry is like the picture on the right side. Then there is nothing you can do to draw graphic elements in this stage. You must apply CTM to the whole graphic elements to place them on LibVGL coordinates system. There are three steps: translate, rotate, and reverse y direction. 
   
![Image](https://github.com/user-attachments/assets/33a5e922-c883-4f31-ade0-4682139de2b7)

![Image](https://github.com/user-attachments/assets/d1174a82-d7be-432f-b227-0b53d82c1add)

4. Results
```Python
import libvgl as vgl
fmm  = vgl.FrameManager()
# sx: 1, sy: 1, wid: 6, hgt: 6, xmin: 0, xmax: 1, ymin: 0, ymax: 1
frm = fmm.create(1,1,6,6, vgl.Data(0,1,0,1))
dev = vgl.DevicePDF("1.pdf", fmm.get_gbbox(),pdir='L',compression=False)
dev.set_device(frm)
dev.line(0,0,.8,.8,lcol=vgl.BLUE, lthk=0.01)
vgl.draw_axis(dev)
dev.close()
```

![Image](https://github.com/user-attachments/assets/e0c7a8f5-ffe8-4d39-aab6-fad2d5dc4d37)

> [!NOTE]
> [https://github.com/uhwang/vgl-core/blob/main/libvgl/vgl/drvpdf.py]
