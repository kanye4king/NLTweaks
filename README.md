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

1. Open NLClientApp.Core

    ![image](https://github.com/kanye4king/NLTweaks/assets/124884528/ee3234a6-2879-47ce-b723-df39ca205ee4)

   ![image](https://github.com/kanye4king/NLTweaks/assets/124884528/8e6ec2e8-df03-4b86-a91b-5bd284b04ca9)

2. Navigate to NLClientApp.Core > NlClientApp.Core.dll > NLClientApp.Core > OnMessage

   ![image](https://github.com/kanye4king/NLTweaks/assets/124884528/783c99d2-6681-46c3-ac5a-f673999d4968)

3. Replace the OnMessage() function with the following code

```
public bool OnMessage(int msg, IntPtr wParam, IntPtr lParam)
{
	if (MainVM.Current.ActiveClient == null)
	{
		return false;
	}
	if (msg == 786)
	{
		int num = wParam.ToInt32();
		if (num >= 61441 && num < 61441 + this.HKIdToRuleId.Count)
		{
			try
			{
				Tuple<HotKey, List<string>> tuple = this.HKIdToRuleId[num - 61441];
				List<Rule> list = new List<Rule>();
				foreach (string b in tuple.Item2)
				{
					foreach (RuleVM ruleVM in MainVM.Current.ActiveClient.Rules)
					{
						if (ruleVM.Model.Id == b)
						{
							ruleVM.IsEnabled = !ruleVM.IsEnabled;
							list.Add(ruleVM.Model);
							foreach (FilterVM filter in MainVM.Current.ActiveClient.Filters)
							{
								if (ruleVM.Model.FilterId == filter.Model.Id)
								{
									try
									{
										using (SoundPlayer player = new SoundPlayer(Path.Combine(Directory.GetCurrentDirectory(), filter.Name) + ruleVM.IsEnabled.ToString() + ".wav"))
										{
											player.Play();
										}
									}
									catch
									{
									}
								}
							}
						}
					}
				}
				foreach (Rule rule in list)
				{
					MainVM.Current.ActiveClient.Model.UpdateRule(rule);
				}
				return true;
			}
			catch (Exception ex)
			{
				UIHelper.ShowError(Application.Current.MainWindow, ex.Message);
			}
			return false;
		}
	}
	return false;
}
```
4. Drag sound effects into the NetLimiter directory, name them in the following structure: 
"{Filter Name}{true/false}.wav"
![image](https://github.com/kanye4king/NLTweaks/assets/124884528/2858a8f2-44eb-4ceb-ab18-00325744e183)
Where true is the sound to play when the rule is ENABLED, and false is the sound to play when the rule is DISABLED
Please ensure that files are in the .wav format


    
   
  



