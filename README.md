options:
	pf: &c&l[!]
	noperms: &c&l[!] &cYou don't have enough permissions.
	
variables:
	{money.%player%} = 0
	
command /bal:
	trigger:
		send "{@pf} &c$%{money.%player%}%"

command /addmoney [<offlineplayer>] [<integer>]:
	permission: money.add
	permission message: {@noperms}
	trigger:
		if arg 1 is not set:
			send "{@pf} &cUsage: /moneyadd <player> <amount>" to sender
			stop
		if arg 2 is not set:
			send "{@pf} &cUsage: /moneyadd <player> <amount>" to sender
			stop
		if arg 2 is set:
			add arg 2 to {money.%arg 1%}
			send "{@pf} &7You transferred &c$%arg 2% &7to &c%arg 1%&7." to sender
			
command /setmoney [<offlineplayer>] [<integer>]:
	permission: money.set
	permission message: {@noperms}
	trigger:
		if arg 1 is not set:
			send "{@pf} &cUsage: /moneyset <player> <amount>" to sender
			stop
		if arg 2 is not set:
			send "{@pf} &cUsage: /moneyset <player> <amount>" to sender
			stop
		if arg 2 is set:
			set {money.%arg 1%} to arg 2
			send "{@pf} &7You have set the money from &c%arg 1% &7to &c$%arg 2%&7." to sender
			
command /pay [<player>] [<integer>]:
	trigger:
		if arg 1 is not set:
			send "{@pf} &cUsage: /pay <player> <amount>" to sender
			stop
		if arg 1 is "%sender%":
			send "{@pf} &cYou can not send yourself money." to sender
			stop
		if arg 2 is not set:
			send "{@pf} &cUsage: /pay <player> <amount>" to sender
			stop
		if arg 2 is set:
			if arg 2 is less than 1:
				send "{@pf} &cYou can not send less than 0 money to someone." to sender
			if arg 2 is bigger than 0:
				if arg 2 is less than {money.%sender%} +1:
					add arg 2 to {money.%arg 1%}
					remove arg 2 from {money.%sender%}
					send "{@pf} &7You have paid &c%arg 1% $%arg 2%&7." to sender
					send "{@pf} &c%sender% &7has paid you &c$%arg 2%&7." to arg 1
				else:
					send "{@pf} &cYou can not send more money to someone like you." to sender
