<template>
	<div>
		<admin-user-dialog />

		<v-data-table
		  :headers="headers"
		  :items="users"
		  class="elevation-1"
		  :loading="loading"
		  :no-data-text="$t('admin.usersTable.empty')"
		>

			<template slot="items" slot-scope="props">
				<td class="text-xs-right">{{ props.item.uid }}</td>
				<td class="text-xs-right">{{ props.item.email }}</td>
				<td class="text-xs-right">{{ props.item.username }}</td>
				<td class="justify-center layout px-0">
					<v-btn class="mx-0" icon @click="editUser(props.item)">
						<v-icon color="teal">edit</v-icon>
					</v-btn>
					<v-btn class="mx-0" icon @click="removeUser(props.item)">
						<v-icon color="pink">delete</v-icon>
					</v-btn>
				</td>
			</template>
		  
		</v-data-table>
	</div>
</template>

<script>
import AdminUserDialog from "@/components/administration/AdminUserDialog";
import { db } from "@/main";
export default {
	name: "AdminUsers",
	components: {
		AdminUserDialog
	},
	data() {
		return {
			headers: [
				{
					text: this.$t("admin.usersTable.uid"),
					value: "uid",
					align: "center"
				},
				{
					text: this.$t("admin.usersTable.email"),
					value: "emal",
					align: "center"
				},
				{
					text: this.$t("admin.usersTable.username"),
					value: "username",
					align: "center"
				},
				{
					text: this.$t("common.actions"),
					value: "name",
					sortable: false
				}
			],
			users: [],
			loading: true
		};
	},
	mounted() {
		this.loading = true;
		db.collection("users").onSnapshot(snapshot => {
			this.users = [];
			snapshot.forEach(user => {
				const userData = user.data();
				this.users.push({
					uid: userData.uid,
					email: userData.email,
					username: userData.username || "-----"
				});
			});
			this.loading = false;
		});
	},
	methods: {
		editUser(user) {
			this.$store.commit("toggleUsersDialog", { editMode: true, user });
		},
		removeUser(user) {
			db.collection("users")
				.doc(user.uid)
				.delete()
				.then(() => {
					this.$store.commit("setAlertMessage", {
						show: true,
						type: "success",
						message: this.$t("messages.deleted", {
							item: this.$t("common.user")
						}),
						timeout: 5000
					});
				});
		}
	}
};
</script>

<style lang="css" scoped>
</style>
