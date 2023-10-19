<script>
	import * as tweetnacl from 'tweetnacl/nacl-fast';
	
	let message = 'Hola me Llamo Jesus';
	let key = tweetnacl.randomBytes(32); // Clave secreta
	let encryptedMessage = ''; // Variable para almacenar el mensaje cifrado
  
	// Función para cifrar el mensaje
	function encryptMessage() {
	  let nonce = tweetnacl.randomBytes(24); // Valor único para cada mensaje
	  encryptedMessage = tweetnacl.secretbox(
		new TextEncoder().encode(message),
		nonce,
		key
	  );
	}
  
	// Función para descifrar el mensaje
	function decryptMessage() {
	  let nonce = tweetnacl.randomBytes(24); // Debe ser el mismo nonce utilizado para cifrar
	  let decryptedMessage = tweetnacl.secretbox.open(
		encryptedMessage,
		nonce,
		key
	  );
	  if (decryptedMessage) {
		encryptedMessage = new TextDecoder().decode(decryptedMessage);
	  } else {
		console.error('Error al descifrar el mensaje');
	  }
	}
  </script>
  
  <main>
	<p>Mensaje a Cifrar:</p>
	<p>{message}</p>
	<button on:click={encryptMessage}>Cifrar Mensaje</button>
	<button on:click={decryptMessage}>Descifrar Mensaje</button>
	{#if encryptedMessage}
	  <p>Mensaje Cifrado o Descifrado: {encryptedMessage}</p>
	{/if}
  </main>
  