group.js
const config = require('../config');

module.exports = {
    name: 'group',
    description: 'Gère les paramètres du groupe (admins seulement)',
    usage: '!group [open/close]',
    
    async execute(sock, msg) {
        // Vérifier si c'est dans un groupe
        if (!msg.isGroup) {
            return await msg.reply('❌ Cette commande fonctionne uniquement dans les groupes');
        }
        
        try {
            // Obtenir les métadonnées du groupe
            const groupMetadata = await sock.groupMetadata(msg.from);
            const participants = groupMetadata.participants;
            
            // Vérifier si l'utilisateur est admin
            const botNumber = sock.user.id.split(':')[0];
            const senderNumber = msg.sender.split('@')[0];
            
            const botParticipant = participants.find(p => p.id.split('@')[0] === botNumber);
            const senderParticipant = participants.find(p => p.id.split('@')[0] === senderNumber);
            
            // Vérifier les permissions
            const isBotAdmin = botParticipant?.admin === 'admin' || botParticipant?.admin === 'superadmin';
            const isSenderAdmin = senderParticipant?.admin === 'admin' || senderParticipant?.admin === 'superadmin';
            
            if (!isSenderAdmin) {
                return await msg.reply('❌ Seuls les administrateurs peuvent utiliser cette commande');
            }
            
            if (!isBotAdmin) {
                return await msg.reply('❌ Je dois être administrateur pour modifier les paramètres du groupe');
            }
            
            // Parser l'action
            const action = msg.args[0]?.toLowerCase();
            
            if (!action || !['open', 'close'].includes(action)) {
                return await msg.reply('❌ Utilisation: !group [open/close]\n\n' +
                                     '• open - Tous les membres peuvent envoyer des messages\n' +
                                     '• close - Seuls les admins peuvent envoyer des messages');
            }
            
            // Modifier les paramètres
            await sock.groupSettingUpdate(msg.from, action === 'close' ? 'announcement' :
