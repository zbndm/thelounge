<template>
	<span class="content">
		<span class="text"><ParsedMessage :network="network" :text="cleanText" /></span>
	</span>
</template>

<script lang="ts">
import {computed, defineComponent, PropType} from "vue";
import {ClientNetwork, ClientMessage} from "../../js/types";
import ParsedMessage from "../ParsedMessage.vue";

export default defineComponent({
	name: "MessageTypeMonospaceBlock",
	components: {
		ParsedMessage,
	},
	props: {
		network: {
			type: Object as PropType<ClientNetwork>,
			required: true,
		},
		message: {
			type: Object as PropType<ClientMessage>,
			required: true,
		},
	},
	setup(props) {
		const cleanText = computed(() => {
			let lines = props.message.text.split("\n");

			// If all non-empty lines of the MOTD start with a hyphen (which is common
			// across MOTDs), remove all the leading hyphens.
			if (lines.every((line) => line === "" || line[0] === "-")) {
				lines = lines.map((line) => line.substring(2));
			}

			// Remove empty lines around the MOTD (but not within it)
			return lines
				.map((line) => line.replace(/\s*$/, ""))
				.join("\n")
				.replace(/^[\r\n]+|[\r\n]+$/g, "");
		});

		return {
			cleanText,
		};
	},
});
</script>
