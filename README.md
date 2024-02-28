# NLTweaks
A handful of tweaks to NetLimtier v4.1.13.0

Drag and drop installations (should) only work with v4.1.13.0, but short copy+paste tutorials will be added below using [dnSpy.exe](https://github.com/dnSpy/dnSpy):

1. Removing enforced modifier hotkeys
   1. Open NLClientApp.Core and NLClientApp.Modules in dnSpy
   ![image](https://github.com/kanye4king/NLTweaks/assets/124884528/ee3234a6-2879-47ce-b723-df39ca205ee4)
   ![image](https://github.com/kanye4king/NLTweaks/assets/124884528/3e1c2e02-2df9-4141-a367-55c8de855f3b)
   2. Navigate to NLClientApp.Modules > NLCLientApp.Modules.HotkeySelector > StackPanel_KeyDown
   ![image](https://github.com/kanye4king/NLTweaks/assets/124884528/f1d8f664-3b93-41fb-8315-348df18c6242)
   3. Replace the StackPanel_KeyDown function with the following code
   private void StackPanel_KeyDown(object sender, KeyEventArgs e)
  {
  	this.Key = e.Key;
  	this.ModifierKeys = Keyboard.Modifiers;
  	this.BtnSet.IsEnabled = true;
  	this.SetComponents();
  }
  



