# kiwiirc-plugin-biolink
Show users Bio Link in kiwiIRC UserBox

IRC allows you to share very little bio data with other users. Using bio.link lets you link to all your pages — websites, social posts, videos, music, bio, photo — making it easier for your audience to discover all your content.

Using [bio.link](https://bio.link) you can also enter a dogecoin tipping jar following [TipMysite](https://www.tipmysite.com) instructions.

This plugin requires Anope IRC service, a custom misc data in ns_set_misc nickserv module and a custom rest API end point to get anope misc data.

example for nickserv.conf

	command { service = "NickServ"; name = "SET BIOLINK"; command = "nickserv/set/misc"; misc_description = _("Associate a BIOLINK url with your account ( https://bio.link )"); }


if you use Magirc for your rest API create a custom API to get misc data :

in magirc rest/service.php add :


	$magirc->slim->get('/nmisc/{user}', function($req, $res, $args) use($magirc) {
    		return $res->withJson($magirc->service->getNMisc($args['user']));
	});

this will add the ns misc API endpoint (example: https://www.example.com/rest/service.php/nmisc/{account} )

in magirc lib/magirc/services/Anope.class.php add :

 
    public function getNMisc($user) {
        
	$query = sprintf("SELECT * FROM anope_db_NSMiscData WHERE nc = :user");

        $ps = $this->db->prepare($query);
        $ps->bindValue(':user', $user, PDO::PARAM_STR);
        $ps->execute();
		$array = array();
		while ($data = $ps->fetch(PDO::FETCH_ASSOC)) {
			$array[ltrim($data['name'], 'ns_set_misc:')] = $data['data'];
		}
		return $array;
    }
 

# Donations

To support this project you can send a donation to the following accounts:

- DOGE: DEqpxyKcz8cEWXA2xobFmot9jCG6TbGWRY


