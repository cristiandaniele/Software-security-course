# Azure setup
Here the steps to use Azure for your fuzzing project (thanks to Patrick Lodeweegs, who is TA for this course).

- Log  in on https://portal.azure.com with your Radboud account
- Click on the side menu to "create a resource"
- One option to create a resource is to use Ubuntu Server 20.04 LTS by clicking on create. However, most other Linux servers should work too.
- On the "create a virtual machine"  screen, some important values to set-up are:
    - Subscription: "Azure for Students"
    - Resource group: Any name
    - Virtual machine name: Any name
    - Region: The chosen region has impact on the price. A relatively cheap location is "(US) West US 2".
    - Image: For instance Ubuntu Server 20.04 LTS.
    - Azure Spot instance: This can be around 50% cheaper. But probably not all fuzzers can handle unexpected interruptions properly.
    - Size: Standard DS1 v2 en A1_v2 both give good performance with afl++ on a toy tested SUT. (This SUT was fuzzgoat. This only a toy example, but you could use it to check if things are working properly.)
    -  For the administrator account adding a public key immediately is a preferred option.
