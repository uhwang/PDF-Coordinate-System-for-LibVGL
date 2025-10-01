# PDF-Coordinate-System-for-LibVGL
Simple PDF driver for LibVGL (Vector Graphic Library)
1. Coordinate system of LibVGL
The coordinate system of LibVGL is the same as Cartesian coordinate system in mathematics except the direction of y-axis. The y-axis extends downwards which is the same as the pixel coordinate system.  
![Image](https://github.com/user-attachments/assets/0b4df8dd-f7cc-4755-91ef-a40125fa6a3a)
PDF coordinate system is the same as Cartesian coordinate system. The origin is left bottom. The x and y limits are the maximum value of paper size. If a paper is a letter, x extends to 8.5 inch in right direction, and y extends to 11 inch in upper direction. The goal of PDF driver is to convert the default coordinate of PDF to LibVGl coordinate. 
2. Understanding PDF Coordinate system.
![Image](https://github.com/user-attachments/assets/c52a6944-3197-42fd-bfc3-c1617b5cecd7)
![Image](https://github.com/user-attachments/assets/f5e91e83-e4fb-40cf-abef-b4fabe365248)
The default coordinate system of PDF is like the picture on the left side. There are two types of page direction: portrait and landscape. For portrait, reversing y direction is enough, but for landscape more steps are necessary to plot graphic elements. For landscape, the page must be rotated 90 degrees. This can be done in page dictionary (3 0 obj at the sample PDF file). When the page is rotated to 90 degrees, the axis geometry is like the picture on the right side. Then there is nothing you can do to draw graphic elements in this stage. You must use CTM to the whole graphic elements to place them on LibVGL coordinates system. There are three steps: translate, rotate, and reverse y direction. 
   
![Image](https://github.com/user-attachments/assets/7d5d599b-5099-4c1d-86f4-80da61e0ad2f)
![Image](https://github.com/user-attachments/assets/4f5c3b18-e6ca-4cfc-91ed-50926651a38d)
![Image](https://github.com/user-attachments/assets/886e725d-3928-4fb5-bc0e-fb81b0b9b292)
 
4. Results
import libvgl as vgl
fmm  = vgl.FrameManager()
frm = fmm.create(1,1,6,6, vgl.Data(0,1,0,1))
dev = vgl.DevicePDF("1.pdf", fmm.get_gbbox(),pdir='L',compression=False)
dev.set_device(frm)
dev.line(0,0,.8,.8,lcol=vgl.BLUE, lthk=0.01)
vgl.draw_axis(dev)
dev.close()
fmm.create(1,1,6,6, …) : a frame with starting point(1 inch in x and 1 inch in y from upper left corner) and width (6 inch), height (6 inch). The frame is the outermost boundary. 
vgl.Data(0,1,0,1) : xmin (0), xmax (1), ymin (0), ymax (1)

![Image](https://github.com/user-attachments/assets/d1174a82-d7be-432f-b227-0b53d82c1add)

 
