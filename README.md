# rcp-customize-fields
// Add field how did you hear about us.
 
function ag_rcp_add_select_field() {

    $referrer = get_user_meta( get_current_user_id(), 'rcp_referrer', true );
    ?>
    <p>
        <label for="rcp_referrer"><?php _e( 'How did you hear about us?', 'rcp' ); ?></label>
        <select id="rcp_referrer" name="rcp_referrer">
            <option value="google" <?php selected( $referrer, 'google'); ?>><?php _e( 'Google', 'rcp' ); ?></option>
            <option value="linkedin" <?php selected( $referrer, 'linkedin'); ?>><?php _e( 'Linkedin', 'rcp' ); ?></option>
            <option value="meetup" <?php selected( $referrer, 'meetup'); ?>><?php _e( 'Local Meetup', 'rcp' ); ?></option>
            <option value="conference" <?php selected( $referrer, 'conference'); ?>><?php _e( 'Conference', 'rcp' ); ?></option>
			<option value="association" <?php selected( $referrer, 'association'); ?>><?php _e( 'Association', 'rcp' ); ?></option>
            <option value="training" <?php selected( $referrer, 'training'); ?>><?php _e( 'Training/Class', 'rcp' ); ?></option>
            <option value="college" <?php selected( $referrer, 'college'); ?>><?php _e( 'College', 'rcp' ); ?></option>
            <option value="friend" <?php selected( $referrer, 'friend'); ?>><?php _e( 'A Friend', 'rcp' ); ?></option>
        </select>
    </p>

    <?php
}

add_action( 'rcp_after_password_registration_field', 'ag_rcp_add_select_field' );
add_action( 'rcp_profile_editor_after', 'ag_rcp_add_select_field' );

/**
 * Adds the custom select field to the member edit screen.
 */
function ag_rcp_add_select_member_edit_field( $user_id = 0 ) {

    $referrer = get_user_meta( $user_id, 'rcp_referrer', true );
    ?>
    <tr valign="top">
        <th scope="row" valign="top">
            <label for="rcp_referrer"><?php _e( 'Referred By', 'rcp' ); ?></label>
        </th>
        <td>
            <select id="rcp_referrer" name="rcp_referrer">
                <option value="google" <?php selected( $referrer, 'google'); ?>><?php _e( 'Google', 'rcp' ); ?></option>
                <option value="linkedin" <?php selected( $referrer, 'linkedin'); ?>><?php _e( 'Linkedin', 'rcp' ); ?></option>
                <option value="meetup" <?php selected( $referrer, 'meetup'); ?>><?php _e( 'Local Meetup', 'rcp' ); ?></option>
                <option value="conference" <?php selected( $referrer, 'conference'); ?>><?php _e( 'Conference', 'rcp' ); ?></option>
				<option value="association" <?php selected( $referrer, 'association'); ?>><?php _e( 'Association', 'rcp' ); ?></option>
                <option value="training" <?php selected( $referrer, 'training'); ?>><?php _e( 'Training/Class', 'rcp' ); ?></option>
                <option value="college" <?php selected( $referrer, 'college'); ?>><?php _e( 'College', 'rcp' ); ?></option>
                <option value="friend" <?php selected( $referrer, 'friend'); ?>><?php _e( 'A Friend', 'rcp' ); ?></option>
            </select>
        </td>
    </tr>
    <?php
}

add_action( 'rcp_edit_member_after', 'ag_rcp_add_select_member_edit_field' );

/**
 * Determines if there are problems with the registration data submitted.
 */
function ag_rcp_validate_select_on_register( $posted ) {

    if ( is_user_logged_in() ) {
        return;
    }
    
    // List all the available options that can be selected.
    $available_choices = array(
        'google',
        'linkedin',
        'meetup',
        'conference',
		'association',
        'training',
        'college',
        'friend'
    );

    // Add an error message if the submitted option isn't one of our valid choices.
    if ( ! in_array( $posted['rcp_referrer'], $available_choices ) ) {
        rcp_errors()->add( 'invalid_referrer', __( 'Please select a valid referrer', 'rcp' ), 'register' );
    }

}

add_action( 'rcp_form_errors', 'ag_rcp_validate_select_on_register', 10 );

/**
 * Stores the information submitted during registration.
 */
function ag_rcp_save_select_field_on_register( $posted, $user_id ) {

    if ( ! empty( $posted['rcp_referrer'] ) ) {
        update_user_meta( $user_id, 'rcp_referrer', sanitize_text_field( $posted['rcp_referrer'] ) );
    }

}

add_action( 'rcp_form_processing', 'ag_rcp_save_select_field_on_register', 10, 2 );

/**
 * Stores the information submitted during profile update.
 */
function ag_rcp_save_select_field_on_profile_save( $user_id ) {
    
    // List all the available options that can be selected.
    $available_choices = array(
        'google',
        'linkedin',
        'meetup',
        'conference',
		'association',
        'training',
        'college',
        'friend'
    );

    if ( isset( $_POST['rcp_referrer'] ) && in_array( $_POST['rcp_referrer'], $available_choices ) ) {
        update_user_meta( $user_id, 'rcp_referrer', sanitize_text_field( $_POST['rcp_referrer'] ) );
    }

}

add_action( 'rcp_user_profile_updated', 'ag_rcp_save_select_field_on_profile_save', 10 );
add_action( 'rcp_edit_member', 'ag_rcp_save_select_field_on_profile_save', 10 );

// End field how did you hear about us.
// 
// Add field What is your role in agile.
 
function ag_rcp_add_select_field_role() {

    $role = get_user_meta( get_current_user_id(), 'rcp_role', true );
    ?>
    <p>
        <label for="rcp_role"><?php _e( 'What is your role in agile?', 'rcp' ); ?></label>
        <select id="rcp_role" name="rcp_role">
            <option value="scrum" <?php selected( $role, 'scrum'); ?>><?php _e( 'Scrum Master', 'rcp' ); ?></option>
            <option value="product" <?php selected( $role, 'product'); ?>><?php _e( 'Product Owner', 'rcp' ); ?></option>
			<option value="project" <?php selected( $role, 'project'); ?>><?php _e( 'Project Lead', 'rcp' ); ?></option>
            <option value="developer" <?php selected( $role, 'developer'); ?>><?php _e( 'Developer', 'rcp' ); ?></option>
            <option value="designer" <?php selected( $role, 'designer'); ?>><?php _e( 'Designer', 'rcp' ); ?></option>
			<option value="agile" <?php selected( $role, 'agile'); ?>><?php _e( 'Agile Coach', 'rcp' ); ?></option>
            <option value="executive" <?php selected( $role, 'executive'); ?>><?php _e( 'Executive', 'rcp' ); ?></option>
            <option value="business" <?php selected( $role, 'business'); ?>><?php _e( 'Business Owner', 'rcp' ); ?></option>
            <option value="entrepreneur" <?php selected( $role, 'entrepreneur'); ?>><?php _e( 'Entrepreneur', 'rcp' ); ?></option>
        </select>
    </p>

    <?php
}

add_action( 'rcp_after_password_registration_field', 'ag_rcp_add_select_field_role' );
add_action( 'rcp_profile_editor_after', 'ag_rcp_add_select_field_role' );

/**
 * Adds the custom select field to the member edit screen.
 */
function ag_rcp_add_select_member_edit_field_role( $user_id = 0 ) {

    $role = get_user_meta( $user_id, 'rcp_role', true );
    ?>
    <tr valign="top">
        <th scope="row" valign="top">
            <label for="rcp_role"><?php _e( 'Role', 'rcp' ); ?></label>
        </th>
        <td>
            <select id="rcp_role" name="rcp_role">
                <option value="scrum" <?php selected( $role, 'scrum'); ?>><?php _e( 'Scrum Master', 'rcp' ); ?></option>
                <option value="product" <?php selected( $role, 'product'); ?>><?php _e( 'Product Owner', 'rcp' ); ?></option>
				<option value="project" <?php selected( $role, 'project'); ?>><?php _e( 'Project Lead', 'rcp' ); ?></option>
                <option value="developer" <?php selected( $role, 'developer'); ?>><?php _e( 'Developer', 'rcp' ); ?></option>
                <option value="designer" <?php selected( $role, 'designer'); ?>><?php _e( 'Designer', 'rcp' ); ?></option>
				<option value="agile" <?php selected( $role, 'agile'); ?>><?php _e( 'Agile Coach', 'rcp' ); ?></option>
                <option value="executive" <?php selected( $role, 'executive'); ?>><?php _e( 'Executive', 'rcp' ); ?></option>
                <option value="business" <?php selected( $role, 'business'); ?>><?php _e( 'Business Owner', 'rcp' ); ?></option>
                <option value="entrepreneur" <?php selected( $role, 'entrepreneur'); ?>><?php _e( 'Entrepreneur', 'rcp' ); ?></option>
            </select>
        </td>
    </tr>
    <?php
}

add_action( 'rcp_edit_member_after', 'ag_rcp_add_select_member_edit_field_role' );

/**
 * Determines if there are problems with the registration data submitted.
 */
function ag_rcp_validate_select_on_register_role( $posted ) {

    if ( is_user_logged_in() ) {
        return;
    }
    
    // List all the available options that can be selected.
    $available_choices = array(
        'google',
        'linkedin',
        'meetup',
        'conference',
		'association',
        'training',
        'college',
        'friend'
    );

    // Add an error message if the submitted option isn't one of our valid choices.
    if ( ! in_array( $posted['rcp_role'], $available_choices ) ) {
        rcp_errors()->add( 'invalid_role', __( 'Please select a valid role', 'rcp' ), 'register' );
    }

}

add_action( 'rcp_form_errors', 'ag_rcp_validate_select_on_register_role', 10 );

/**
 * Stores the information submitted during registration.
 */
function ag_rcp_save_select_field_on_register_role( $posted, $user_id ) {

    if ( ! empty( $posted['rcp_role'] ) ) {
        update_user_meta( $user_id, 'rcp_role', sanitize_text_field( $posted['rcp_role'] ) );
    }

}

add_action( 'rcp_form_processing', 'ag_rcp_save_select_field_on_register', 10, 2 );

/**
 * Stores the information submitted during profile update.
 */
function ag_rcp_save_select_field_on_profile_save_role( $user_id ) {
    
    // List all the available options that can be selected.
    $available_choices = array(
        'google',
        'linkedin',
        'meetup',
        'conference',
		'association',
        'training',
        'college',
        'friend'
    );

    if ( isset( $_POST['rcp_role'] ) && in_array( $_POST['rcp_role'], $available_choices ) ) {
        update_user_meta( $user_id, 'rcp_role', sanitize_text_field( $_POST['rcp_role'] ) );
    }

}

add_action( 'rcp_user_profile_updated', 'ag_rcp_save_select_field_on_profile_save_role', 10 );
add_action( 'rcp_edit_member', 'ag_rcp_save_select_field_on_profile_save_role', 10 );
