<?php
/**
 * Plugin Name: FaucetPay
 * Plugin URI: https://faucetpay.io
 * Description: Provides shortcodes for Crypto Faucet (BCH, BLK, BTC, DASH, DOGE, ETH, LTC)
 * Version: 1.00
 * Author: FaucetPay.io
 * Author URI: https://faucetpay.io
 * Text Domain: 99btc-bf
 * Contributors: FaucetPay <hi@faucetpay.io>
 */

if (!class_exists('The99Btcfaucetpay')) {
    require_once __DIR__ . '/libraries/faucetpay.php';

}
if (!class_exists('The99BtcSolveMediaResponse')) {
    require_once __DIR__ . '/libraries/solvemedialib.php';
}
if (!function_exists('the99btc_sqn_validate')) {
    require_once __DIR__ . '/libraries/sqn.php';
}

if (!class_exists('The99Bitcoins_BtcFaucet_Ban_Addresses')) {

    require_once __DIR__ . '/src/Client/BitCoinCore.php';

    require_once __DIR__ . '/src/Ban/Addresses.php';
    require_once __DIR__ . '/src/Ban/Ips.php';

    require_once __DIR__ . '/src/Claim/Ips.php';
    require_once __DIR__ . '/src/Claim/Payouts.php';
    require_once __DIR__ . '/src/Claim/Stats.php';

    require_once __DIR__ . '/src/ClaimRules/Base.php';
    require_once __DIR__ . '/src/ClaimRules/BaseExchangeRate.php';
    require_once __DIR__ . '/src/ClaimRules/BTC.php';
    require_once __DIR__ . '/src/ClaimRules/DASH.php';
    require_once __DIR__ . '/src/ClaimRules/DOGE.php';
    require_once __DIR__ . '/src/ClaimRules/ETH.php';
    require_once __DIR__ . '/src/ClaimRules/LTC.php';
    require_once __DIR__ . '/src/ClaimRules/BCH.php';
    require_once __DIR__ . '/src/ClaimRules/USDBTC.php';
    require_once __DIR__ . '/src/ClaimRules/USDDASH.php';
    require_once __DIR__ . '/src/ClaimRules/USDDOGE.php';
    require_once __DIR__ . '/src/ClaimRules/USDETH.php';
    require_once __DIR__ . '/src/ClaimRules/USDLTC.php';
    require_once __DIR__ . '/src/ClaimRules/USDBCH.php';


    require_once __DIR__ . '/src/Currency/Base.php';
    require_once __DIR__ . '/src/Currency/BTC.php';
    require_once __DIR__ . '/src/Currency/DASH.php';
    require_once __DIR__ . '/src/Currency/DOGE.php';
    require_once __DIR__ . '/src/Currency/ETH.php';
    require_once __DIR__ . '/src/Currency/LTC.php';
    require_once __DIR__ . '/src/Currency/BCH.php';


    require_once __DIR__ . '/src/Info/Addresses.php';
    require_once __DIR__ . '/src/Info/Ips.php';
    require_once __DIR__ . '/src/Info/Users.php';

    require_once __DIR__ . '/src/Scheduled/Payment.php';
    require_once __DIR__ . '/src/Scheduled/Payouts.php';

    require_once __DIR__ . '/src/Tool/Kv.php';

    require_once __DIR__ . '/src/Wallet/WalletInterface.php';

    require_once __DIR__ . '/src/Wallet/Fake.php';
    require_once __DIR__ . '/src/Wallet/faucetpay.php';

    require_once __DIR__ . '/src/Migration/Changes.php';
    require_once __DIR__ . '/src/Manager.php';
    require_once __DIR__ . '/src/Plugin.php';
}

function The99BitcoinsBtcFaucetCron()
{
    global $the99BitcoinsBtcFaucet;
    if ($the99BitcoinsBtcFaucet) {
        $the99BitcoinsBtcFaucet->cron();
    }
}

function The99BitcoinsBtcFaucetInitialization()
{
	global $the99BitcoinsBtcFaucet;
	if (!function_exists('get_plugin_data')) {
		require_once ABSPATH . 'wp-admin/includes/plugin.php';
	}
    $plugin = get_plugin_data(__FILE__);

	if (!$the99BitcoinsBtcFaucet) {
		$the99BitcoinsBtcFaucet = new The99Bitcoins_BtcFaucet_Manager(array(
            'root' => plugin_dir_path(__FILE__),
            'plugin_url' => plugin_dir_url(__FILE__),
            'templates' => plugin_dir_path(__FILE__) . '/templates/',
            'version' => $plugin['Version'],
		));
	}

	register_activation_hook(__FILE__, array($the99BitcoinsBtcFaucet, 'install'));
	register_deactivation_hook(__FILE__, array($the99BitcoinsBtcFaucet, 'uninstall'));
}

add_action('The99BitcoinsBtcFaucetCron', 'The99BitcoinsBtcFaucetCron');
add_action('plugins_loaded', 'The99BitcoinsBtcFaucetInitialization');

/** @var The99Bitcoins_BtcFaucet_Manager $the99BitcoinsBtcFaucet */
