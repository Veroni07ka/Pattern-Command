using System;

// ----- Command Interface -----
interface ICommand
{
    void Execute();
    void Undo();
}

// ----- Receiver -----
class Light
{
    public void TurnOn()
    {
        Console.WriteLine("💡 The light is ON.");
    }

    public void TurnOff()
    {
        Console.WriteLine("💡 The light is OFF.");
    }
}

// ----- Concrete Commands -----
class TurnOnLightCommand : ICommand
{
    private Light _light;

    public TurnOnLightCommand(Light light)
    {
        _light = light;
    }

    public void Execute()
    {
        _light.TurnOn();
    }

    public void Undo()
    {
        _light.TurnOff();
    }
}

class TurnOffLightCommand : ICommand
{
    private Light _light;

    public TurnOffLightCommand(Light light)
    {
        _light = light;
    }

    public void Execute()
    {
        _light.TurnOff();
    }

    public void Undo()
    {
        _light.TurnOn();
    }
}

// ----- Invoker -----
class RemoteControl
{
    private ICommand _command;

    public void SetCommand(ICommand command)
    {
        _command = command;
    }

    public void PressButton()
    {
        Console.WriteLine("🔘 Pressing button...");
        _command.Execute();
    }

    public void PressUndo()
    {
        Console.WriteLine("↩️ Undoing last command...");
        _command.Undo();
    }
}

// ----- Client -----
class Program
{
    static void Main(string[] args)
    {
        // Create receiver
        Light light = new Light();

        // Create commands
        ICommand lightOn = new TurnOnLightCommand(light);
        ICommand lightOff = new TurnOffLightCommand(light);

        // Create invoker
        RemoteControl remote = new RemoteControl();

        // Execute ON command
        remote.SetCommand(lightOn);
        remote.PressButton();

        // Execute OFF command
        remote.SetCommand(lightOff);
        remote.PressButton();

        // Undo last action
        remote.PressUndo();
    }
}
