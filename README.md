# Design-Patterns
## C# Adapter


O padrão de design Adapter converte a interface de uma classe em outra interface que os clientes esperam. Esse padrão de design permite que classes trabalhem juntas que não poderiam de outra forma devido a interfaces incompatíveis.

Frequência de uso: ![image](https://user-images.githubusercontent.com/60439056/195726826-1db58f61-d95f-4975-873c-3a234a97c9cc.png) médio-alto

## UML diagrama de classes
Uma visualização das classes e objetos que participam desse padrão.

![image](https://user-images.githubusercontent.com/60439056/195726890-e135f95c-1d57-4dcc-9742-ce6a3484e70b.png)

## Participants
As classes e objetos que participam desse padrão incluem:

* Target   (ChemicalCompound)
  * define a interface específica do domínio que o Cliente usa.

* Adapter   (Compound)
  * adapta a interface Adaptee à interface Target.

* Adaptee   (ChemicalDatabank)
  * define uma interface existente que precisa ser adaptada.

* Client   (AdapterApp)
  * colabora com objetos em conformidade com a interface Target.
  
## Código estrutural em C#

```C#
using System;

namespace Adapter.Structural
{
    /// <summary>
    /// Adapter Design Pattern
    /// </summary>

    public class Program
    {
        public static void Main(string[] args)
        {
            // Create adapter and place a request

            Target target = new Adapter();
            target.Request();

            // Wait for user

            Console.ReadKey();
        }

    }

    /// <summary>
    /// The 'Target' class
    /// </summary>

    public class Target
    {
        public virtual void Request()
        {
            Console.WriteLine("Called Target Request()");
        }
    }

    /// <summary>
    /// The 'Adapter' class
    /// </summary>

    public class Adapter : Target
    {
        private Adaptee adaptee = new Adaptee();

        public override void Request()
        {
            // Possibly do some other work
            // and then call SpecificRequest

            adaptee.SpecificRequest();
        }
    }

    /// <summary>
    /// The 'Adaptee' class
    /// </summary>

    public class Adaptee
    {
        public void SpecificRequest()
        {
            Console.WriteLine("Called SpecificRequest()");
        }
    }
}
```

### Output 
```
Called SpecificRequest()
```

## Código do mundo real em C#
Este código do mundo real demonstra o uso de um banco de dados químico legado. Objetos de compostos químicos acessam o banco de dados por meio de uma interface Adapter.

```C#
using System;

namespace Adapter.RealWorld
{
    /// <summary>
    /// Adapter Design Pattern
    /// </summary>

    public class Program
    {
        public static void Main(string[] args)
        {
            // Non-adapted chemical compound

            Compound unknown = new Compound();
            unknown.Display();

            // Adapted chemical compounds

            Compound water = new RichCompound("Water");
            water.Display();

            Compound benzene = new RichCompound("Benzene");
            benzene.Display();

            Compound ethanol = new RichCompound("Ethanol");
            ethanol.Display();

            // Wait for user

            Console.ReadKey();
        }
    }

    /// <summary>
    /// The 'Target' class
    /// </summary>

    public class Compound
    {
        protected float boilingPoint;
        protected float meltingPoint;
        protected double molecularWeight;
        protected string molecularFormula;

        public virtual void Display()
        {
            Console.WriteLine("\nCompound: Unknown ------ ");
        }
    }

    /// <summary>
    /// The 'Adapter' class
    /// </summary>

    public class RichCompound : Compound
    {
        private string chemical;
        private ChemicalDatabank bank;

        // Constructor

        public RichCompound(string chemical)
        {
            this.chemical = chemical;
        }

        public override void Display()
        {
            // The Adaptee

            bank = new ChemicalDatabank();

            boilingPoint = bank.GetCriticalPoint(chemical, "B");
            meltingPoint = bank.GetCriticalPoint(chemical, "M");
            molecularWeight = bank.GetMolecularWeight(chemical);
            molecularFormula = bank.GetMolecularStructure(chemical);

            Console.WriteLine("\nCompound: {0} ------ ", chemical);
            Console.WriteLine(" Formula: {0}", molecularFormula);
            Console.WriteLine(" Weight : {0}", molecularWeight);
            Console.WriteLine(" Melting Pt: {0}", meltingPoint);
            Console.WriteLine(" Boiling Pt: {0}", boilingPoint);
        }
    }

    /// <summary>
    /// The 'Adaptee' class
    /// </summary>

    public class ChemicalDatabank
    {
        // The databank 'legacy API'

        public float GetCriticalPoint(string compound, string point)
        {
            // Melting Point
            if (point == "M")
            {
                switch (compound.ToLower())
                {
                    case "water": return 0.0f;
                    case "benzene": return 5.5f;
                    case "ethanol": return -114.1f;
                    default: return 0f;
                }
            }

            // Boiling Point

            else
            {
                switch (compound.ToLower())
                {
                    case "water": return 100.0f;
                    case "benzene": return 80.1f;
                    case "ethanol": return 78.3f;
                    default: return 0f;
                }
            }
        }

        public string GetMolecularStructure(string compound)
        {
            switch (compound.ToLower())
            {
                case "water": return "H20";
                case "benzene": return "C6H6";
                case "ethanol": return "C2H5OH";
                default: return "";
            }
        }

        public double GetMolecularWeight(string compound)
        {
            switch (compound.ToLower())
            {
                case "water": return 18.015;
                case "benzene": return 78.1134;
                case "ethanol": return 46.0688;
                default: return 0d;
            }
        }
    }
}
```

### Resultado
```
Composto: Desconhecido ------

Composto: Água ------
 Fórmula: H20
 Peso: 18,015 Ponto de
 fusão: 0 Ponto de
 ebulição: 100

Composto: Benzeno ------
 Fórmula: C6H6
 Peso: 78,1134 Ponto de
 fusão: 5,5 Ponto de
 ebulição: 80,1

Composto: Álcool ------
 Fórmula: C2H6O2
 Peso: 46,0688 Ponto de
 fusão: -114,1 Ponto de
 ebulição: 78,3
```
