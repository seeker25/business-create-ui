<template>
  <div id="name-translation">
    <!-- Name Translation form -->
    <v-form v-model="nameTranslationForm" class="name-translation-form">
      <v-row>
        <v-col class="pb-0">
          <v-text-field
            filled
            persistent-hint
            id="name-translation-input"
            label="Enter Name Translation"
            v-model="nameTranslation"
            :rules="nameTranslationRules"
          />
        </v-col>
      </v-row>
      <v-row>
        <v-col class="form__btns pt-0">
          <v-btn large color="error" id="btn-remove"
            :disabled="!editNameTranslation"
            @click="removeTranslation()"
          >
            Remove
          </v-btn>
          <v-btn large color="primary" id="btn-done" class="form-primary-btn"
            :disabled="!nameTranslationForm"
            @click="addTranslation()"
          >
            Done
          </v-btn>
          <v-btn large id="btn-cancel" class="form-cancel-btn"
            @click="cancelTranslation()"
          >
            Cancel
          </v-btn>
        </v-col>
      </v-row>
    </v-form>
  </div>
</template>

<script lang="ts">
// Libraries
import { Component, Emit, Prop, Vue } from 'vue-property-decorator'

@Component({})
export default class AddNameTranslation extends Vue {
  @Prop({ default: '' })
  readonly editNameTranslation: string

  // Local properties
  private nameTranslationForm: boolean = false
  private nameTranslation: string = ''

  // Validation rules
  readonly nameTranslationRules: Array<Function> = [
    v => !!v || 'A name translation is required', // is not empty
    v => /^[A-Za-zÀ-ÿ_@./#’&+-]+(?: [A-Za-zÀ-ÿ_@./#’&+-]+)*$/.test(v) || 'Invalid character', // English, French and single spaces
    v => (!v || v.length <= 150) || 'Cannot exceed 150 characters' // maximum character count
  ]

  mounted (): void {
    // Editing an existing name translation
    if (this.editNameTranslation) this.nameTranslation = this.editNameTranslation
  }

  // Events
  @Emit('addTranslation')
  private addTranslation (): string {
    return this.nameTranslation
  }

  @Emit('cancelTranslation')
  private cancelTranslation (): void {}

  @Emit('removeTranslation')
  private removeTranslation (): void {}
}
</script>
