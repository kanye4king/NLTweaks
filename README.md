# NLTweaks
A handful of tweaks to NetLimiter v4.1.13.0
More Tweaks will be added over time!

Drag and drop installations (should) only work with v4.1.13.0, but short copy+paste tutorials will be added below using [dnSpy.exe](https://github.com/dnSpy/dnSpy):

# Removing Enforced Modifier Keys
   1. Open NLClientApp.Core and NLClientApp.Modules in dnSpy
   ![image](https://github.com/kanye4king/NLTweaks/assets/124884528/ee3234a6-2879-47ce-b723-df39ca205ee4)

   ![image](https://github.com/kanye4king/NLTweaks/assets/124884528/3e1c2e02-2df9-4141-a367-55c8de855f3b)
   
   3. Navigate to NLClientApp.Modules > NLCLientApp.Modules.HotkeySelector > StackPanel_KeyDown
   ![image](https://github.com/kanye4king/NLTweaks/assets/124884528/f1d8f664-3b93-41fb-8315-348df18c6242)

   5. Replace the StackPanel_KeyDown function with the following code
   
  ```
   private void StackPanel_KeyDown(object sender, KeyEventArgs e)
      {
        	this.Key = e.Key;
        	this.ModifierKeys = Keyboard.Modifiers;
        	this.BtnSet.IsEnabled = true;
        	this.SetComponents();
     }
```
   (Everything from here on is optional to get rid of "None + {HotKey}")

   6. Replace the SetComponents function with the following code (it should be two function above the StackPanel_KeyDown function in the same file)
```
      private void SetComponents()
		{
			if (this.Key == Key.None)
			{
				this.BtnDelete.IsEnabled = false;
				return;
			}
			if (this.ModifierKeys.ToString() == "None")
			{
				this.TxtBlock.Text = this.Key.ToString();
			}
			else
			{
				this.TxtBlock.Text = this.ModifierKeys.ToString() + "+" + this.Key.ToString();
			}
			this.BtnDelete.IsEnabled = true;
		}
```
   8. Navigate to NLClientApp.Core > HotKey

   ![image](https://github.com/kanye4king/NLTweaks/assets/124884528/f308ff14-3dad-4a64-8b05-125ac21458f3)

   9. Replace the ToString() ovveride with the following code
```
      public override string ToString()
		{
			if (this.ModifierKeys.ToString() == "None")
			{
				return this.Key.ToString();
			}
			return this.ModifierKeys.ToString() + "+" + this.Key.ToString();
		}
```
# Sound Effects on Hotkey Press
	1. hello
   
  



