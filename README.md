import arcade
import random
import time
import diagonal_lib
import math

W = 2500
H = 1200
N = 5
#a-это угол 

def grad_to_rad(grad):
    return grad * math.pi / 180

class Shar(): #обьект шар 
    def __init__ (self, x_, y_, v_, a_, c_):
        self.x=x_
        self.y=y_
        self.v=v_
        self.a=a_
        self.c=c_

        self.u=0
        self.check_gor =False

    def Draw(self): #рисуется обьект шар 
        arcade.draw_point(self.x, self.y,self.c, 10 )
       
    def Move(self):#движение шаров !!!!
        vy=self.v*math.sin(grad_to_rad(self.a))
        vx=self.v*math.cos(grad_to_rad(self.a))
        
        self.x += vx
        self.y += vy
            
    def ykxplusb(self,    line_,     x_):#функция которая 
        k =(line_.yy -line_.y)/(line_.xx-line_.x)
        b =line_.y
        dx = line_.x
        y=k*(x_-dx)+b
        return y
        
    def peresechenie_up_to_down(self, line_):#пересечение сверху вниз
        B=90-line_.a
        self.a=180-(B+B+self.a)

    def peresechenie_up_to_down22(self, line_):#пересечение сверху вниз
        self.a=180+(180-self.a)
        
    def Durka(self, x__, y__, c):
        
        if self.x>x__ and self.x- 15<=x__ and y__-300<self.y<y__:
            #print("1")
            #del
            self.c=(255, 255, 255)
            
            
    def Durka_Draw(self, x_, y_, c):
        
        arcade.draw_point(x_, y_, c, 10)
        arcade.draw_point(x_,y_-300, c, 10)
        
    def obschet_otr_sharov(self, line_):#определение какую функцию вызывать
        y = self.ykxplusb(line_ , self.x)
            
        if self.y>y and self.y- 15<=y and line_.x<self.x<line_.xx:
            #print("касаются")
            self.peresechenie_up_to_down(line_)
            self.u=1
            #print("rwqgp];,g")

            
            
        if self.y<y and self.y+15>=y and line_.x<self.x<line_.xx:
            #print("касаются")
            self.peresechenie_up_to_down(line_)
            self.u=1
            #print("iojhn")

        
class Orbite(arcade.Window):

    def __init__(self, w_, h_, wind_):
        super().__init__(w_, h_)
        self.w=wind_
           
        self.s=[]
    
        self.t = 0
        self.lines = []
        self.durka=[]
        self.h=random.randint(0, W)
        self.hh=random.randint(0, H)
        self.ran=0
        self.u=0
        self.ran=random.randint(5,10)
        self.ran2=random.randint(-10,-5)

    def setup(self):
        arcade.set_background_color(arcade.color.WHITE)
        self.lines.append(diagonal_lib.Line(300,400,1000,900))
        self.lines.append(diagonal_lib.Line(800,100,1500,1100))
        
        #print(self.lines[0].a)
        
       
    def on_draw(self):
        arcade.start_render()
        for shar in self.s:
            shar.Draw()
            shar.Durka_Draw(self.h, self.hh, (76, 65, 34) )

        for line in self.lines:
            line.line_Draw()
  
        for durka in self.durka:
            durka.Draw()

    def on_key_press(self, symbol, modifier):
        
        if symbol == arcade.key.W:#1
            self.lines[0].y +=10
            
        if symbol == arcade.key.D:#1
            self.lines[0].x +=10
            
        if symbol == arcade.key.A:#1
            self.lines[0].x -=10
            
        if symbol == arcade.key.S:#1
            self.lines[0].y -=10


        if symbol == arcade.key.I:#1
            self.lines[0].yy +=10
            
        if symbol == arcade.key.L:#1
            self.lines[0].xx +=10
            
        if symbol == arcade.key.J:#1
            self.lines[0].xx -=10
            
        if symbol == arcade.key.K:#1
            self.lines[0].yy -=10

        if symbol == arcade.key.T:#2
            self.lines[1].y +=10
            
        if symbol == arcade.key.H:#2
            self.lines[1].x +=10
            
        if symbol == arcade.key.F:#2
            self.lines[1].x -=10
            
        if symbol == arcade.key.G:#2
            self.lines[1].y -=10


        if symbol == arcade.key.UP:#2
            self.lines[1].yy +=10
            
        if symbol == arcade.key.RIGHT:#2
            self.lines[1].xx +=10
            
        if symbol == arcade.key.LEFT:#2
            self.lines[1].xx -=10
            
        if symbol == arcade.key.DOWN:#2
            self.lines[1].yy -=10

     
    def update(self, delta_time):
           
        for shar in self.s:
            shar.Move()
            #for i in range(1, 2):
            shar.obschet_otr_sharov(self.lines[0])
            shar.obschet_otr_sharov(self.lines[1])
            shar.Durka(self.h, self.hh, (87, 65, 34) )
        
            if shar.u==1:
                #self.s.append(Shar(shar.x, shar.y, shar.v, 90+self.lines[0].a, (207, 107, 169)	))
                #if shar.v>0:
                  #  shar.u=0
                #else:
                 #   self.s.append(Shar(shar.x, shar.y, shar.v, self.lines[0].a+shar.a, (78, 65, 37)))
                  #  shar.u=0
                shar.u=0

                
        self.t +=1

        if self.t%30 ==0:
            if random.randint(1,2)==1:
                
                self.s.append(Shar(random.randint(0, W), random.randint(0, H), self.ran, random.randint(-45, 45), (0, 0, 0)))
            else:
                
                self.s.append(Shar(random.randint(0, W), random.randint(0, H), self.ran2, random.randint(-45, 45), (0, 0, 0)))
            

def main():
    O=Orbite(W, H, 0)
    O.setup()
    arcade.run()
    

if __name__=="__main__":
    main()

