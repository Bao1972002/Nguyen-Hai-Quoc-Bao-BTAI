# Nguyen-Hai-Quoc-Bao-BTAI
from tkinter import *
from tkinter import filedialog as fd
from PIL import Image, ImageTk
import numpy as np
from keras.utils import img_to_array,load_img
from keras.models import load_model
#Khai mở windown
gui = Tk()
gui.geometry('600x550')
gui.title('Nhận diện sâu bệnh trên cây táo dại')
def image():
    global img
    filename = fd.askopenfilename(initialdir="/images",title='Chọn ảnh',filetypes=file_type)
    url['text'] = filename
    img = Image.open(filename)
    img = img.resize((200,200))
    img = ImageTk.PhotoImage(img)
    show_image['image'] = img

def predict():
    label = ['scab','multiple_diseases','healthy','rust']
    model = load_model('nhan_dien_benh_tren_cay_taodai.h5')
    img= load_img(url.cget('text'),target_size=(200,200))
    img=img_to_array(img)
    img=img.astype('float32')
    img=img.reshape(1,200,200,3)
    img=img/255
    a= int(np.argmax(model.predict(img),axis=1))
    show_predict['text'] = label[a]


file_type = [('Tất cả file','.*'),('Ảnh','.png')]
title = Label(gui,text='Phần mền dự đoán sâu bệnh',font=("Arial Bold",30),width=25)
title.grid(row=0,column=0)
url = Label(gui,font=("Arial Bold",10),width=30)
url.grid(row=2,column=0)
button1 = Button(gui,text='Image selection',font=("Arial Bold",20),width=20,command=image)
button1.grid(row=4,column=0)
button2 = Button(gui,text='Predict',font=("Arial Bold",20),width=20,command=predict)
button2.grid(row=5,column=0)
show_image = Label(gui)
show_image.grid(row=6,column=0)
show_predict = Label(gui,font=("Arial Bold",20),text='',width=20)
show_predict.grid(row=8,column=0)

gui.mainloop()
