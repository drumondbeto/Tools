# Sealed Classes and Members

## Sealed Modifier
- Prevents derivation of classes and overriding of methods.


        public sealed class Circle : Shape
        {
            public override void Draw()
            {
                Console.WriteLine("Draw a circle");
            }
        }

        public class Circle : Shape
        {
            public sealed override void Draw()
            {
                Console.WriteLine("Draw a circle");
            }
        }