# Abstract Classes and Members

- An abstract class may have abstract members, but an abstract member must belong to an abstract class;


        public abstract class Shape
        {
            public abstract void Draw();

            public void Copy()
            {
                Console.WriteLine("Copy shape into clipboard.");
            }
        }


- When we have an daughter class from an abstract class, we need to override the abstract method if we want do call it:


        public class Circle : Shape
        {
            public void Draw()
            {
                Console.WriteLine("Draw a circle")
            }
        }

