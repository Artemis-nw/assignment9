def parse_config(config_string, required_settings):
    if config_string[-1] != '>':
        return "Error: Incomplete configuration."

    config_string = config_string[:-1]
    smths = config_string.split('--')
    sum = {}
    for smth in smths:
        if '::' in smth:
            room = smth.split('::')
            gg = room[0]
            value = room[1]
            sum[gg] = value
    missing_keys = []
    for gg in required_settings:
        if gg not in sum:
            missing_keys.append(gg)

    if missing_keys:
        return "Error: Missing settings: " + str(missing_keys)
    output = []
    for gg in required_settings:
        output.append(sum[gg])

    return output
conf1 = "SSID::GuestNet--PASS::Secret123--IP::DHCP>"
req1 = ["SSID", "PASS", "IP"]
print(parse_config(conf1, req1))  

conf2 = "SSID::HomeWifi--CHANNEL::6>"
req2 = ["SSID", "PASS"]
print(parse_config(conf2, req2))  

conf3 = "SSID::Office--PASS::Admin"
req3 = ["SSID"]
print(parse_config(conf3, req3))  

conf4 = "TIMEOUT::30--PORT::8080--HOST::Localhost>"
req4 = ["HOST", "PORT", "TIMEOUT"]
print(parse_config(conf4, req4))
