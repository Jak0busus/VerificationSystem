# VerificationSystem
VerificationSystemPlugin [limbo - 1.20]\
⚠ This is just a summary of the features and the appearance of the plugin, it is not a public project!



<!-- IMAGES -->
## Images

Discord Embed\
![embed]
![key_embed]
![already_registered]\
Motd\
![wrong_version]
![motd]\
Minecraft Title\
![paste_key]
![is_linked]\
Actionbar\
![already_linked]\
![wrong_pattern]\
![key_is_invalid]



<!-- USAGE EXAMPLES -->
## Usage

The plugin runs on a custom Paper fork designed for lobbies to optimize performance. The API is nearly identical, allowing the code to be utilized for Paper as well. The purpose of the plugin is to link Discord accounts with Minecraft accounts. To achieve this, users can input `/embed` in a designated Discord channel to create an embed explaining the verification process, complete with a button to request a verification code. Alternatively, `/verify` serves as an option to trigger this process. Subsequently, a random code is generated and stored in the database along with the corresponding Discord ID. Upon the player joining the Minecraft server, they will be prompted to input the code. Furthermore, the Minecraft server can be configured based on the desired version to display custom MOTDs. The plugin also includes action bar messages, which, by default, utilize a custom font.

When the correct code is input, the two accounts are linked and recorded together in the database. This linkage allows accessibility for transactions, events, and more that require the combined usage of Discord and Minecraft. All database queries are fully asynchronous, ensuring no lag occurs. All messages, embeds, and sounds are customizable within the comprehensive configuration file.



## Code

I used IntelliJ as the IDE. I utilized Gradle with the Kotlin build script to add dependencies.
  ```sh
    implementation("com.loohp:Limbo:0.7.5-ALPHA")
    implementation("net.kyori:adventure-text-minimessage:4.14.0")
    implementation("net.dv8tion:JDA:5.0.0-beta.12")
    implementation("de.chojo.sadu", "sadu", "1.3.1")
    implementation("org.mariadb.jdbc:mariadb-java-client:3.1.4")
    implementation("com.zaxxer:HikariCP:5.0.1")

    implementation("ch.qos.logback:logback-classic:1.2.8")
    implementation("org.slf4j:slf4j-log4j12:2.0.7")
  ```
The Sadu library simplifies sending asynchronous database queries using HikariCP. 
  ```sh
        public CompletableFuture<Boolean> updateMinecraftUUID(String verificationKey, String minecraftUUID) {
        return builder()
                .query("""
                        UPDATE account
                        SET minecraft_uuid = ?,
                        verification_key = NULL WHERE verification_key = ?
                         """)
                .parameter(stmt -> stmt.setString(minecraftUUID).setString(verificationKey))
                .update()
                .send()
                .thenApply(UpdateResult::changed);
    }
  ```
The configuration employs MiniMessages messages to provide maximum configurability.\
https://docs.advntr.dev/minimessage/api.html



### Commands

`/embed` `/verify`

### Permissions

`bot` `applications.commands` `messages.send` `embed.links`

### Configuration

  ```sh
# Database
host: ""
port: 3306
database: ""
# The plugin will automatically create a table named 'account' to manage the data
username: ""
password: ""


# Discord Bot
token: ""
guild: ""
channel: ""

# Verification Key
key:
  characters: "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"
  length: 8

# Minecraft
title:
  already_verified:
    text: "<color:#FBC716>Account is linked"
  not_verified:
    text: "<color:#FBC716>Paste key in chat"

message:
  already_verified:
    text: "<color:grey>Your Account is linked with <color:#FBC716><username>"

  not_verified:
    text: "<color:grey>Please enter your <color:#70BA11>verification key"

actionbar:
  already_verified:
    text: "<color:#FBC716>ᴀʟʀᴇᴀᴅʏ ʟɪɴᴋᴇᴅ!"
    sound: ""

  wrong_key_pattern:
    text: "<color:#D4193E>ɴᴏᴛ ᴀ ᴠᴇʀɪꜰɪᴄᴀᴛɪᴏɴ ᴋᴇʏ!"
    sound: ""

  invalid_verification_key:
    text: "<color:#D4193E>ᴠᴇʀɪꜰɪᴄᴀᴛɪᴏɴ ᴋᴇʏ ɪꜱ ɪɴᴠᴀʟɪᴅ!"
    sound: ""

  successfully_verified:
    text: "<color:#70BA11>ꜱᴜᴄᴄᴇꜱꜱꜰᴜʟʟʏ ʟɪɴᴋᴇᴅ!"
    sound: "minecraft:entity.player.levelup"

music:
  join_song: "minecraft:music.menu"

# Discord
embed:
  verification_info:
    content:
      title: "Verification"
      description: "Use the `/verify` command or click on the button below to effortlessly link your Discord and Minecraft account. Once invoked, you'll receive an exclusive verification code. Simply input this code ingame on the Minecraft server to establish a connection between your Discord and Minecraft profiles.\nIf you've already obtained a verification code, don't worry! Just enter the command again to receive a brand new code."
      footer: "Server Ip:"
      color: "#70BA11"
    button:
      emoji: "U+1F517"
      text: "get key"

  verification_key:
    content:
      title: "`<key>`"
      description: "Enter this key on the Minecraft server to link your accounts. If you already had a code, it has now been overridden by this one. For assistance, please contact the support."
      footer: "Server Ip:"
      color: "#70BA11"

  already_verified:
    content:
      title: "`Already Registered`"
      description: "It appears that your Discord account is already connected to your Minecraft account. If you believe this is an error or have any concerns, please don't hesitate to contact our support."
      footer: "Linked with <player>"
      color: "#C84043"

command:

  embed:
    description: "Connect your Discord account to your Minecraft account."

  verify:
    description: "Creates an embed."
  ```



<!-- CONTACT -->
## Contact

Github: [@Jak0busus](https://github.com/Jak0busus)
Discord: [Jakob#2006](https://discord.com/users/580369300041498635)



<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->

[embed]: images/embed.png
[key_embed]: images/key_embed.png
[wrong_version]: images/wrong_version.png
[motd]: images/motd.png
[paste_key]: images/paste_key.png
[wrong_pattern]: images/wrong_pattern.png
[key_is_invalid]: images/key_is_invalid.png
[already_registered]: images/already_registered.png
[is_linked]: images/is_linked.png
[already_linked]: images/already_linked.png
